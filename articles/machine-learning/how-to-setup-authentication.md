---
title: 设置身份验证
titleSuffix: Azure Machine Learning
description: 了解如何为 Azure 机器学习中的各种资源和工作流设置和配置身份验证。
services: machine-learning
author: cjgronlund
ms.author: cgronlun
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.date: 11/05/2020
ms.topic: conceptual
ms.custom: how-to, has-adal-ref, devx-track-js, devx-track-azurecli, contperfq2
ms.openlocfilehash: 7fa6beacf4456145e312494a72dad321dfef3754
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843921"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>为 Azure 机器学习资源和工作流设置身份验证


了解如何设置 Azure 机器学习工作区的身份验证。 Azure 机器学习工作区的身份验证基于大多数情况 __Azure Active Directory__ (Azure AD) 。 通常，在连接到工作区时，可以使用三个身份验证工作流：

* __交互式__：你可以使用 Azure Active Directory 中的帐户直接进行身份验证，或者使用它来获取用于身份验证的令牌。 在 _试验和迭代开发_ 期间使用交互式身份验证。 交互式身份验证使你能够根据每个用户的需要控制对资源 (（如 web 服务) ）的访问。

* __服务主体__：在 Azure Active Directory 中创建一个服务主体帐户，并使用它来进行身份验证或获取令牌。 当你需要 _自动过程向服务进行身份验证_ 而无需用户交互时，可以使用服务主体。 例如连续集成和部署脚本，它可以在训练代码每次发生更改时对模型进行训练和测试。

* __托管标识__：在 _azure 虚拟机上_ 使用 Azure 机器学习 SDK 时，可以使用 azure 的托管标识。 此工作流允许 VM 使用托管标识连接到工作区，而无需在 Python 代码中存储凭据或提示用户进行身份验证。 还可以将 Azure 机器学习计算群集配置为在 _训练模型_ 时使用托管标识访问工作区。

> [!IMPORTANT]
> 无论使用何种身份验证工作流，Azure RBAC)  (基于角色的访问控制可用于确定允许访问资源的访问权限级别 (授权) 范围。 例如，管理员或自动化过程可能具有创建计算实例的权限，但不使用它，而数据科学家可以使用它，但不能删除或创建计算实例。 有关详细信息，请参阅[管理对 Azure 机器学习工作区的访问权限](how-to-assign-roles.md)。

## <a name="prerequisites"></a>先决条件

* 创建 [Azure 机器学习工作区](how-to-manage-workspace.md)。
* [配置开发环境](how-to-configure-environment.md) 以安装 Azure 机器学习 sdk，或使用已安装 sdk 的 [Azure 机器学习计算实例](concept-azure-machine-learning-architecture.md#compute-instance) 。

## <a name="azure-active-directory"></a>Azure Active Directory

工作区的所有身份验证工作流都依赖于 Azure Active Directory。 如果希望用户使用个人帐户进行身份验证，则他们必须在 Azure AD 中有帐户。 如果要使用服务主体，则它们必须位于 Azure AD 中。 托管标识也是 Azure AD 的一项功能。 

有关 Azure AD 的详细信息，请参阅 [什么是 Azure Active Directory 身份验证](..//active-directory/authentication/overview-authentication.md)。

创建 Azure AD 帐户后，请参阅 [管理对 Azure 机器学习工作区的访问权限](how-to-assign-roles.md) ，以获取有关向其授予对工作区的访问权限以及 Azure 机器学习中其他操作的信息。

## <a name="configure-a-service-principal"></a>配置服务主体

若要使用服务主体 (SP) ，必须首先创建 SP，并向其授予对工作区的访问权限。 如前所述，Azure RBAC)  (Azure 基于角色的访问控制可用于控制访问权限，因此，你还必须确定授予 SP 的访问权限。

> [!IMPORTANT]
> 使用服务主体时，请向它授予它所用于的 __任务所需的最低访问权限__。 例如，如果服务主体仅用于读取 Web 部署的访问令牌，则不要向服务主体授予所有者或参与者访问权限。
>
> 授予最低访问权限的原因是服务主体使用密码进行身份验证，并且该密码可以存储为自动化脚本的一部分。 如果密码泄漏，由于用户仅拥有执行特定任务所需的最低访问权限，因此可最大程度地减少对 SP 的恶意使用。

创建 SP 并向其授予对工作区的访问权限的最简单方法是使用 [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest)。 若要创建服务主体并向其授予对工作区的访问权限，请执行以下步骤：

> [!NOTE]
> 你必须是订阅的管理员才能执行所有这些步骤。

1. 对 Azure 订阅进行身份验证：

    ```azurecli-interactive
    az login
    ```

    如果 CLI 可以打开默认的浏览器，则它会打开该浏览器并加载登录页。 否则，需要打开浏览器并按照命令行中的说明操作。 按说明操作时，需要浏览到 [https://aka.ms/devicelogin](https://aka.ms/devicelogin) 并输入授权代码。

    如果有多个 Azure 订阅，可以使用 `az account set -s <subscription name or ID>` 命令来设置订阅。 有关详细信息，请参阅[使用多个 Azure 订阅](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)。

    有关其他身份验证方法，请参阅[使用 Azure CLI 登录](/cli/azure/authenticate-azure-cli?preserve-view=true&view=azure-cli-latest)。

1. 安装 Azure 机器学习扩展：

    ```azurecli-interactive
    az extension add -n azure-cli-ml
    ```

1. 创建服务主体。 在以下示例中，将创建一个名为 **ml-auth** 的 SP：

    ```azurecli-interactive
    az ad sp create-for-rbac --sdk-auth --name ml-auth
    ```

    输出将是类似于如以下所示的 JSON。 记下 `clientId`、`clientSecret` 和 `tenantId` 字段，因为在本文的其他步骤中你需要用到它们。

    ```json
    {
        "clientId": "your-client-id",
        "clientSecret": "your-client-secret",
        "subscriptionId": "your-sub-id",
        "tenantId": "your-tenant-id",
        "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
        "resourceManagerEndpointUrl": "https://management.azure.com",
        "activeDirectoryGraphResourceId": "https://graph.windows.net",
        "sqlManagementEndpointUrl": "https://management.core.windows.net:5555",
        "galleryEndpointUrl": "https://gallery.azure.com/",
        "managementEndpointUrl": "https://management.core.windows.net"
    }
    ```

1. 使用上一步返回的 `clientId` 值检索服务主体的详细信息：

    ```azurecli-interactive
    az ad sp show --id your-client-id
    ```

    下面的 JSON 是命令的输出的简化示例。 记下 `objectId` 字段，因为你将在接下来的步骤中用到它的值。

    ```json
    {
        "accountEnabled": "True",
        "addIns": [],
        "appDisplayName": "ml-auth",
        ...
        ...
        ...
        "objectId": "your-sp-object-id",
        "objectType": "ServicePrincipal"
    }
    ```

1. 允许 SP 访问你的 Azure 机器学习工作区。 你将需要工作区名称，以及分别针对 `-w` 和 `-g` 参数的资源组名称。 对于 `--user` 参数，请使用 `objectId` 之前步骤中的 值。 `--role` 参数用于为服务主体设置访问角色。 在以下示例中，将向 SP 分配 **所有者** 角色。 

    > [!IMPORTANT]
    > 所有者访问权限允许服务主体在工作区中执行几乎所有操作。 本文档中使用它来演示如何授予访问权限；在生产环境中，Microsoft 建议你仅向服务主体授予行使目标角色职能所需的最低访问权限。 若要了解如何使用方案所需的访问权限创建自定义角色，请参阅 [管理对 Azure 机器学习工作区的访问权限](how-to-assign-roles.md)。

    ```azurecli-interactive
    az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
    ```

    此调用在成功时不会生成任何输出。

## <a name="configure-a-managed-identity"></a>配置托管标识

> [!IMPORTANT]
> 仅当使用 Azure 虚拟机中的 Azure 机器学习 SDK 或使用 Azure 机器学习计算群集时，才支持托管标识。 将托管标识用于计算群集目前处于预览阶段。

### <a name="managed-identity-with-a-vm"></a>具有 VM 的托管标识

1. [为 VM 上的 Azure 资源启用系统分配的托管标识](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#system-assigned-managed-identity)。

1. 在 [Azure 门户](https://portal.azure.com)中，选择你的工作区，然后选择 "__访问控制 (IAM)__"、"__添加角色分配__"，然后从 "__分配访问权限__" 下拉列表中选择 "__虚拟机__"。 最后，选择 VM 的标识。

1. 选择要分配给此标识的角色。 例如，参与者或自定义角色。 有关详细信息，请参阅 [控制对资源的访问](how-to-assign-roles.md)。

### <a name="managed-identity-with-compute-cluster"></a>具有计算群集的托管标识

有关详细信息，请参阅 [设置计算群集的托管标识](how-to-create-attach-compute-cluster.md#managed-identity)。

<a id="interactive-authentication"></a>

## <a name="use-interactive-authentication"></a>使用交互式身份验证

> [!IMPORTANT]
> 交互式身份验证使用浏览器并需要 Cookie（包括第三方 Cookie）。 如果已禁用 Cookie，则可能会收到“我们无法为你登录”之类的错误。 如果已启用 [Azure AD 多重身份验证](../active-directory/authentication/concept-mfa-howitworks.md)，则也可能发生此错误。

文档和样本中的大多数示例都使用交互式身份验证。 例如，当使用 SDK 时，有两个函数调用会自动提示你使用基于 UI 的身份验证流：

* 调用 `from_config()` 函数会触发提示。

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

    函数 `from_config()` 查找包含工作区连接信息的 JSON 文件。

* 使用 `Workspace` 构造函数来提供订阅、资源组和工作区信息时，系统也会提示你进行交互式身份验证。

    ```python
    ws = Workspace(subscription_id="your-sub-id",
                  resource_group="your-resource-group-id",
                  workspace_name="your-workspace-name"
                  )
    ```

> [!TIP]
> 如果你可访问多个租户，则可能需要导入类并显式定义目标租户。 与上述调用类似，为 `InteractiveLoginAuthentication` 调用构造函数也会提示你登录。
>
> ```python
> from azureml.core.authentication import InteractiveLoginAuthentication
> interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
> ```

使用 Azure CLI 时，使用 `az login` 命令对 CLI 会话进行身份验证。 有关详细信息，请参阅 [Azure CLI 入门](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。

> [!TIP]
> 如果你使用的是以前使用 Azure CLI 以交互方式进行身份验证的环境中的 SDK，则可以使用 `AzureCliAuthentication` 类通过 CLI 缓存的凭据对工作区进行身份验证：
>
> ```python
> from azureml.core.authentication import AzureCliAuthentication
> cli_auth = AzureCliAuthentication()
> ws = Workspace(subscription_id="your-sub-id",
>                resource_group="your-resource-group-id",
>                workspace_name="your-workspace-name",
>                auth=cli_auth
>                )
> ```

<a id="service-principal-authentication"></a>

## <a name="use-service-principal-authentication"></a>使用服务主体身份验证

若要从 SDK 对工作区进行身份验证，请使用服务主体，并使用 `ServicePrincipalAuthentication` 类构造函数。 使用创建服务提供程序时获得的值作为参数。 `tenant_id` 参数映射到上述的 `tenantId`，`service_principal_id` 映射到 `clientId`，`service_principal_password` 映射到 `clientSecret`。

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

`sp` 变量现在包含身份验证对象，可直接在 SDK 中使用。 通常，建议将上述 ID/机密存储在环境变量中，如下面的代码所示。 存储在环境变量中可防止将信息意外地签入到 GitHub 存储库中。

```python
import os

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

对于在 Python 中运行并主要使用 SDK 的自动化工作流，在大多数情况下，都可按原样使用此对象进行身份验证。 以下代码使用你创建的身份验证对象向你的工作区进行身份验证。

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example",
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

### <a name="use-a-service-principal-from-the-azure-cli"></a>从 Azure CLI 中使用服务主体

可以将服务主体用于 Azure CLI 命令。 有关详细信息，请参阅[使用服务主体登录](/cli/azure/create-an-azure-service-principal-azure-cli?preserve-view=true&view=azure-cli-latest#sign-in-using-a-service-principal)。

### <a name="use-a-service-principal-with-the-rest-api-preview"></a>将服务主体用于 REST API（预览版）

还可以使用服务主体向 Azure 机器学习 [REST API](/rest/api/azureml/)（预览版）进行身份验证。 如果使用 Azure Active Directory [客户端凭据授予流](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md)，则可在自动化工作流中进行无外设身份验证的服务到服务调用。 这些示例使用 Python 和 Node.js 中的 [ADAL 库](../active-directory/azuread-dev/active-directory-authentication-libraries.md)实现，但也可以使用支持 OpenID Connect 1.0 的任何开放源代码库。

> [!NOTE]
> MSAL.js 库比 ADAL 更新，但不能使用 MSAL.js 的客户端凭据进行服务到服务的身份验证，因为它主要是一个客户端库，适用于与特定用户关联的交互式/UI 身份验证。 我们建议使用如下所示的 ADAL，通过 REST API 生成自动化工作流。

#### <a name="nodejs"></a>Node.js

使用以下步骤，通过 Node.js 生成身份验证令牌。 在你的环境中运行 `npm install adal-node`。 然后，使用在上述步骤中创建的服务主体中的 `tenantId`、`clientId` 和 `clientSecret`，作为以下脚本中匹配变量的值。

```javascript
const adal = require('adal-node').AuthenticationContext;

const authorityHostUrl = 'https://login.microsoftonline.com/';
const tenantId = 'your-tenant-id';
const authorityUrl = authorityHostUrl + tenantId;
const clientId = 'your-client-id';
const clientSecret = 'your-client-secret';
const resource = 'https://management.azure.com/';

const context = new adal(authorityUrl);

context.acquireTokenWithClientCredentials(
  resource,
  clientId,
  clientSecret,
  (err, tokenResponse) => {
    if (err) {
      console.log(`Token generation failed due to ${err}`);
    } else {
      console.dir(tokenResponse, { depth: null, colors: true });
    }
  }
);
```

变量 `tokenResponse` 是包含令牌和关联元数据（如过期时间）的对象。 令牌的有效时间为 1 个小时，可以通过再次运行相同的调用检索一个新令牌来对令牌进行刷新。 下面的代码片段是示例响应。

```javascript
{
    tokenType: 'Bearer',
    expiresIn: 3599,
    expiresOn: 2019-12-17T19:15:56.326Z,
    resource: 'https://management.azure.com/',
    accessToken: "random-oauth-token",
    isMRRT: true,
    _clientId: 'your-client-id',
    _authority: 'https://login.microsoftonline.com/your-tenant-id'
}
```

使用 `accessToken` 属性获取身份验证令牌。 请参阅 [REST API 文档](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API)，获取如何使用令牌进行 API 调用的示例。

#### <a name="python"></a>Python

使用以下步骤，通过 Python 生成身份验证令牌。 在你的环境中运行 `pip install adal`。 然后，使用在上述步骤中创建的服务主体中的 `tenantId`、`clientId` 和 `clientSecret`，作为以下脚本中适合的变量的值。

```python
from adal import AuthenticationContext

client_id = "your-client-id"
client_secret = "your-client-secret"
resource_url = "https://login.microsoftonline.com"
tenant_id = "your-tenant-id"
authority = "{}/{}".format(resource_url, tenant_id)

auth_context = AuthenticationContext(authority)
token_response = auth_context.acquire_token_with_client_credentials("https://management.azure.com/", client_id, client_secret)
print(token_response)
```

变量 `token_response` 是包含令牌和关联元数据（如过期时间）的字典。 令牌的有效时间为 1 个小时，可以通过再次运行相同的调用检索一个新令牌来对令牌进行刷新。 下面的代码片段是示例响应。

```python
{
    'tokenType': 'Bearer',
    'expiresIn': 3599,
    'expiresOn': '2019-12-17 19:47:15.150205',
    'resource': 'https://management.azure.com/',
    'accessToken': 'random-oauth-token',
    'isMRRT': True,
    '_clientId': 'your-client-id',
    '_authority': 'https://login.microsoftonline.com/your-tenant-id'
}
```

使用 `token_response["accessToken"]` 获取身份验证令牌。 请参阅 [REST API 文档](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API)，获取如何使用令牌进行 API 调用的示例。

#### <a name="java"></a>Java

在 Java 中，使用标准 REST 调用检索持有者令牌：

```java
String tenantId = "your-tenant-id";
String clientId = "your-client-id";
String clientSecret = "your-client-secret";
String resourceManagerUrl = "https://management.azure.com";

HttpRequest tokenAuthenticationRequest = tokenAuthenticationRequest(tenantId, clientId, clientSecret, resourceManagerUrl);

HttpClient client = HttpClient.newBuilder().build();
Gson gson = new Gson();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
if (response.statusCode == 200)
{
     body = gson.fromJson(body, AuthenticationBody.class);

    // ... etc ... 
}
// ... etc ...

static HttpRequest tokenAuthenticationRequest(String tenantId, String clientId, String clientSecret, String resourceManagerUrl){
    String authUrl = String.format("https://login.microsoftonline.com/%s/oauth2/token", tenantId);
    String clientIdParam = String.format("client_id=%s", clientId);
    String resourceParam = String.format("resource=%s", resourceManagerUrl);
    String clientSecretParam = String.format("client_secret=%s", clientSecret);

    String bodyString = String.format("grant_type=client_credentials&%s&%s&%s", clientIdParam, resourceParam, clientSecretParam);

    HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create(authUrl))
            .POST(HttpRequest.BodyPublishers.ofString(bodyString))
            .build();
    return request;
}

class AuthenticationBody {
    String access_token;
    String token_type;
    int expires_in;
    String scope;
    String refresh_token;
    String id_token;
    
    AuthenticationBody() {}
}
```

前面的代码需要处理除之外的异常和状态代码 `200 OK` ，但会显示模式： 

- 使用客户端 ID 和机密来验证你的程序是否应具有访问权限
- 使用你的租户 ID 指定 `login.microsoftonline.com` 应查看的位置
- 使用 Azure 资源管理器作为授权令牌的源

## <a name="use-managed-identity-authentication"></a>使用托管标识身份验证

若要从使用托管标识配置的 VM 或计算群集对工作区进行身份验证，请使用 `MsiAuthentication` 类。 下面的示例演示如何使用此类对工作区进行身份验证：

```python
from azureml.core.authentication import MsiAuthentication

msi_auth = MsiAuthentication()

ws = Workspace(subscription_id="your-sub-id",
                resource_group="your-resource-group-id",
                workspace_name="your-workspace-name",
                auth=msi_auth
                )
```

## <a name="next-steps"></a>后续步骤

* [如何在训练中使用机密](how-to-use-secrets-in-runs.md)。
* [如何为部署为 web 服务的模型配置身份验证](how-to-authenticate-web-service.md)。
* [使用部署为 Web 服务的 Azure 机器学习模型](how-to-consume-web-service.md)。