---
title: 适用于 CI/CD 的 GitHub 操作
titleSuffix: Azure Machine Learning
description: 了解如何创建 GitHub 操作工作流以在 Azure 机器学习上定型模型
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: juliakm
ms.author: jukullam
ms.date: 10/19/2020
ms.topic: conceptual
ms.custom: github-actions-azure
ms.openlocfilehash: 4336827dc7f8cb45f04e4cef94d79d1e6409d5c0
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92795113"
---
# <a name="use-github-actions-with-azure-machine-learning"></a>在 Azure 机器学习中使用 GitHub 操作

[GitHub 操作](https://help.github.com/en/articles/about-github-actions)入门，可在 Azure 机器学习上训练模型。 

> [!NOTE]
> 适用于 Azure 机器学习的 GitHub 操作按原样提供，不完全受 Microsoft 支持。 如果在执行特定操作时遇到问题，请在存储库中打开此操作的问题。 例如，如果你遇到 aml-部署操作的问题，请在存储库中报告问题 [https://github.com/Azure/aml-deploy]( https://github.com/Azure/aml-deploy) 。

## <a name="prerequisites"></a>先决条件 

- 具有活动订阅的 Azure 帐户。 [免费创建帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
- 一个 GitHub 帐户。 如果没有，请 [免费](https://github.com/join)注册。  

## <a name="workflow-file-overview"></a>工作流文件概述

工作流通过存储库的 `/.github/workflows/` 路径中的 YAML (.yml) 文件定义。 此定义包含组成工作流的各种步骤和参数。

此文件包含四个部分：

|部分  |任务  |
|---------|---------|
|**身份验证** | 1.定义服务主体。 <br /> 2.创建 GitHub 机密。 |
|**“连接”** | 1. 连接到机器学习工作区。 <br /> 2. 连接到计算目标。 |
|**运行** | 1. 提交训练运行。 |
|**部署** | 1. 在 Azure 机器学习注册表中注册模型。 1. 部署模型。 |

## <a name="create-repository"></a>创建存储库

[使用 GitHub 操作和 Azure 机器学习模板，从 ML Ops](https://github.com/machine-learning-apps/ml-template-azure)创建新的存储库。 

1. 打开 GitHub 上的 [模板](https://github.com/machine-learning-apps/ml-template-azure) 。 
2. 选择“使用此模板”  。 

    :::image type="content" source="media/how-to-github-actions-machine-learning/gh-actions-use-template.png" alt-text="选择 &quot;使用此模板&quot;":::
3. 通过模板创建新的存储库。 将存储库名称设置为 `ml-learning` 或选择名称。 


## <a name="generate-deployment-credentials"></a>生成部署凭据

可以使用 [Azure CLI](/cli/azure/) 中的 [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac&preserve-view=true) 命令创建[服务主体](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object)。 请使用 Azure 门户中的 [Azure Cloud Shell](https://shell.azure.com/) 或选择“试用”按钮运行此命令。

```azurecli-interactive
az ad sp create-for-rbac --name "myML" --role contributor \
                            --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name> \
                            --sdk-auth
```

在上面的示例中，将占位符替换为你的订阅 ID、资源组名称和应用名称。 输出是一个具有角色分配凭据的 JSON 对象，该对象提供对应用服务应用的访问权限，如下所示。 稍后复制此 JSON 对象。

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

## <a name="configure-the-github-secret"></a>配置 GitHub 机密

1. 在 [GitHub](https://github.com/)中，浏览存储库，选择 " **设置" > 机密 > 添加新机密** 。

2. 将 Azure CLI 命令的整个 JSON 输出粘贴到机密的值字段中。 为机密指定名称 `AZURE_CREDENTIALS`。

## <a name="connect-to-the-workspace"></a>连接到工作区

使用 [Azure 机器学习工作区操作](https://github.com/marketplace/actions/azure-machine-learning-workspace) 连接到 Azure 机器学习工作区。 

```yaml
    - name: Connect/Create Azure Machine Learning Workspace
      id: aml_workspace
      uses: Azure/aml-workspace@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
```

默认情况下，操作需要一个 `workspace.json` 文件。 如果 JSON 文件具有不同的名称，则可以使用输入参数来指定它 `parameters_file` 。 如果没有文件，将使用存储库名称创建一个新的文件。


```yaml
    - name: Connect/Create Azure Machine Learning Workspace
      id: aml_workspace
      uses: Azure/aml-workspace@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
          parameters_file: "alternate_workspace.json"
```
操作会将工作区 Azure 资源管理器 (ARM) 属性写入配置文件，这将由将来的所有 Azure 机器学习 GitHub 操作选取。 文件将保存到 `GITHUB_WORKSPACE/aml_arm_config.json` 。 

## <a name="connect-to-a-compute-target-in-azure-machine-learning"></a>连接到 Azure 机器学习中的计算目标

使用 [Azure 机器学习计算操作](https://github.com/Azure/aml-compute) 连接到 Azure 机器学习中的计算目标。  如果计算目标存在，则操作将连接到该目标。 否则，该操作将创建新的计算目标。 [AML 计算操作](https://github.com/Azure/aml-compute)仅支持 azure ML 计算群集和 Azure Kubernetes 服务 (AKS) 。 

```yaml
    - name: Connect/Create Azure Machine Learning Compute Target
      id: aml_compute_training
      uses: Azure/aml-compute@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
```
## <a name="submit-training-run"></a>提交定型运行任务

使用 [Azure 机器学习定型操作](https://github.com/Azure/aml-run) 提交要 Azure 机器学习的 ScriptRun、估计器或管道。 

```yaml
    - name: Submit training run
      id: aml_run
      uses: Azure/aml-run@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
```

## <a name="register-model-in-registry"></a>在注册表中注册模型

使用 [Azure 机器学习注册模型操作](https://github.com/Azure/aml-registermodel) 来注册要 Azure 机器学习的模型。

```yaml
    - name: Register model
      id: aml_registermodel
      uses: Azure/aml-registermodel@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
          run_id:  ${{ steps.aml_run.outputs.run_id }}
          experiment_name: ${{ steps.aml_run.outputs.experiment_name }}
```

## <a name="deploy-model-to-azure-machine-learning-to-aci"></a>将模型部署到 Azure 机器学习到 ACI

使用 [Azure 机器学习部署操作](https://github.com/Azure/aml-deploy) 来部署模型，并为模型创建终结点。 你还可以使用 Azure 机器学习部署部署到 Azure Kubernetes 服务。 有关部署到 Azure Kubernetes 服务的模型，请参阅 [此示例工作流](https://github.com/Azure-Samples/mlops-enterprise-template) 。

```yaml
    - name: Deploy model
      id: aml_deploy
      uses: Azure/aml-deploy@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
          model_name:  ${{ steps.aml_registermodel.outputs.model_name }}
          model_version: ${{ steps.aml_registermodel.outputs.model_version }}

```

## <a name="complete-example"></a>完整示例

训练模型，并将其部署到 Azure 机器学习。 

```yaml
# Actions train a model on Azure Machine Learning
name: Azure Machine Learning training and deployment
on:
  push:
    branches:
      - master
    # paths:
    #   - 'code/*'
jobs:
  train:
    runs-on: ubuntu-latest
    steps:
    # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
    - name: Check Out Repository
      id: checkout_repository
      uses: actions/checkout@v2
        
    # Connect or Create the Azure Machine Learning Workspace
    - name: Connect/Create Azure Machine Learning Workspace
      id: aml_workspace
      uses: Azure/aml-workspace@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
    
    # Connect or Create a Compute Target in Azure Machine Learning
    - name: Connect/Create Azure Machine Learning Compute Target
      id: aml_compute_training
      uses: Azure/aml-compute@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
    
    # Submit a training run to the Azure Machine Learning
    - name: Submit training run
      id: aml_run
      uses: Azure/aml-run@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}

    # Register model in Azure Machine Learning model registry
    - name: Register model
      id: aml_registermodel
      uses: Azure/aml-registermodel@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
          run_id:  ${{ steps.aml_run.outputs.run_id }}
          experiment_name: ${{ steps.aml_run.outputs.experiment_name }}

    # Deploy model in Azure Machine Learning to ACI
    - name: Deploy model
      id: aml_deploy
      uses: Azure/aml-deploy@v1
      with:
          azure_credentials: ${{ secrets.AZURE_CREDENTIALS }}
          model_name:  ${{ steps.aml_registermodel.outputs.model_name }}
          model_version: ${{ steps.aml_registermodel.outputs.model_version }}

```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组和存储库，请通过删除资源组和 GitHub 存储库来清理部署的资源。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 Azure 机器学习 SDK 创建和运行机器学习管道](how-to-create-your-first-pipeline.md)
