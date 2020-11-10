---
title: 为 Azure Red Hat OpenShift 4 配置内置容器注册表
description: 为 Azure Red Hat OpenShift 4 配置内置容器注册表
author: jiangma
ms.author: jiangma
ms.service: container-service
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 917da58c7f5ac2294fc30cd385a4834fde3f5f0b
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94414347"
---
# <a name="configure-built-in-container-registry-for-azure-red-hat-openshift-4"></a>为 Azure Red Hat OpenShift 4 配置内置容器注册表

本文介绍如何为 Azure Red Hat OpenShift (ARO) 4 群集配置内置容器映像注册表。 将了解如何执行以下操作：

> [!div class="checklist"]
> * 设置 Azure AD
> * 设置 OpenID Connect
> * 访问内置容器映像注册表

## <a name="prerequisites"></a>必备知识

* 按照 [创建 Azure Red Hat OpenShift 4 群集](/azure/openshift/tutorial-create-cluster)中的步骤创建群集。 请确保创建具有参数的群集 `--pull-secret` `az aro create` 。  如果你想要设置用于登录的 Azure AD，则这是必需的。
* 按照 [连接到 Azure Red Hat OpenShift 4 群集](/azure/openshift/tutorial-connect-cluster)中的步骤连接到群集。
   * 请务必遵循 "安装 OpenShift CLI" 中的步骤，因为我们将 `oc` 在本文的后面部分使用该命令。
   * 记下群集控制台 URL，如所示 `https://console-openshift-console.apps.<random>.<region>.aroapp.io/` 。 `<random>` `<region>` 本文后面将使用和的值。
   * 记下 `kubeadmin` 凭据。 本文稍后将用到它们。

## <a name="set-up-azure-active-directory"></a>设置 Azure Active Directory

Azure Active Directory (Azure AD) 实现 OpenID Connect (OIDC) 。 OIDC 使你可以使用 Azure AD 登录到 ARO 群集。 请按照以下步骤设置 Azure AD。

1. 按照 [快速入门：设置租户](/azure/active-directory/develop/quickstart-create-new-tenant)中的步骤设置 Azure AD 租户。 你的 Azure 帐户可能已有租户。 如果是这样，则可以跳过创建，前提是租户中有足够的特权来执行这些步骤。 请注意你的 **租户 ID** 。 稍后将使用此值。
2. 按照 [使用 Azure Active Directory 添加或删除用户](/azure/active-directory/fundamentals/add-users-azure-active-directory)中的步骤操作，至少创建一个 Azure AD 用户。 你可以使用这些帐户或你自己的帐户来测试应用程序。 记下这些帐户的电子邮件地址和密码，以便登录。
3. 按照 [快速入门：向 Microsoft 标识平台注册应用程序](/azure/active-directory/develop/quickstart-register-app)中的步骤，在 Azure AD 租户中创建新的应用程序注册。 
   1. 请记住有关和值的先决条件中的注释 `<random>` `<region>` 。 根据以下公式，使用这些值创建 URI：

      `https://oauth-openshift.apps.<random>.<region>.aroapp.io/oauth2callback/openid`
   1. 当你到达处理 " **重定向 URI** " 字段的步骤时，输入上一步中的 URI。
   1. 选择“注册”  。
   1. 在应用的 " **概述** " 页上，记下 " **应用程序 (客户端) ID** " 的显示值，如下所示。
      :::image type="content" source="media/built-in-container-registry/azure-ad-app-overview-client-id.png" alt-text="Azure AD 应用程序的 &quot;概述&quot; 页。":::

4. 创建新的客户端密钥。 
   1. 在新创建的应用程序注册中，在左侧导航窗格中选择 " **证书 & 机密** "。
   1. 选择“新建客户端机密”。
   1. 提供 **说明** ，并选择 " **添加** "。
   1. 保留生成的 **客户端机密** 值。 此值将在本文稍后使用。

### <a name="add-openid-connect-identity-provider"></a>添加 OpenID Connect 标识提供者

ARO 提供内置的容器注册表。  若要对其进行访问，请按照以下步骤使用 Azure AD OIDC (IDP) 配置标识提供者。

1. 从浏览器以的身份登录到 OpenShift web 控制台 `kubeadmin` 。  执行 [教程：连接到 Azure Red Hat OpenShift 4 群集](/azure/openshift/tutorial-connect-cluster)中的步骤时，使用的过程与此相同。
1. 在左侧导航窗格中，选择 " **管理**  >  **群集设置** "。
1. 在页面的中间，选择 " **全局配置** "。
1. 找到表的 " **配置资源** " 列中的 " **OAuth** "，并选择它。
1. 在 " **标识提供者** " 下，选择 " **添加** " 并选择 " **OpenID connect** "。
1. 将 **名称** 设置为 **openid** 。  此值可能已填充。
1. 将 **客户端 ID** 和 **客户端密码** 设置为上一步中的值。
1. 按照此步骤查找 " **颁发者 URL** " 的值。
   1. 替换 **\<tenant-id>** 为在 URL 中 **设置 Azure Active Directory** 的部分中保存的 `https://login.microsoftonline.com/<tenant-id>/v2.0/.well-known/openid-configuration` 。
   1. 在用于与 Azure 门户进行交互的同一浏览器中打开 URL。
   1. 复制返回的 JSON 正文中的 "属性 **颁发者** " 的值，并将其粘贴到 " **颁发者 URL** " 文本框中。  **颁发者** 值将如下所示 `https://login.microsoftonline.com/44p4o433-2q55-474q-on88-4on94469o74n/v2.0` 。
1. 转到页面底部，选择 " **添加** "。
   :::image type="content" source="media/built-in-container-registry/openid-connect-identity-provider.png" alt-text="OpenShift 中的 OpenID Connect。":::
1. 在浏览器窗口的右上方选择 " **kube： admin** " 按钮，然后选择 " **注销** "，注销 OpenShift web 控制台。

### <a name="access-the-built-in-container-image-registry"></a>访问内置容器映像注册表

以下说明启用对内置注册表的访问。

#### <a name="define-the-azure-ad-user-to-be-an-administrator"></a>将 Azure AD 用户定义为管理员

1. 使用 Azure AD 用户的凭据，从浏览器登录到 OpenShift web 控制台。

   1. 使用 InPrivate、Incognito 或其他等效的浏览器窗口功能登录到控制台。
   1. 启用 OIDC 后，窗口的外观将有所不同。
   :::image type="content" source="media/built-in-container-registry/oidc-enabled-login-window.png" alt-text="OpenID Connect 已启用登录窗口。":::
   1. 选择 **openid**

   > [!NOTE]
   > 记下用于登录的用户名和密码。 此用户名和密码将充当此文章和其他文章中其他操作的管理员。
1. 使用以下步骤登录到 OpenShift CLI。  对于讨论，此过程称为 `oc login` 。
   1. 在 web 控制台的右上角，展开已登录用户的上下文菜单，然后选择 " **复制登录命令** "。
   1. 如果需要，请使用相同的用户登录到新的选项卡窗口。
   1. 选择 " **显示令牌** "。
   1. 将以下 **使用此令牌登录** 的值复制到剪贴板并在 shell 中运行，如下所示。

       ```bash
       oc login --token=XOdASlzeT7BHT0JZW6Fd4dl5EwHpeBlN27TAdWHseob --server=https://api.aqlm62xm.rnfghf.aroapp.io:6443
       Logged into "https://api.aqlm62xm.rnfghf.aroapp.io:6443" as "kube:admin" using the token provided.

       You have access to 57 projects, the list has been suppressed. You can list all projects with 'oc projects'

       Using project "default".
       ```

1. `oc whoami`在控制台中运行，并记下输出为 **\<aad-user>** 。  本文稍后将用到此值。
1. 从 OpenShift web 控制台注销。 选择标记为的浏览器窗口右上方的按钮 **\<aad-user>** ，然后选择 " **注销** "。


#### <a name="grant-the-azure-ad-user-the-necessary-roles-for-registry-interaction"></a>向 Azure AD 用户授予注册表交互所需的角色

1. 使用凭据登录到浏览器中的 OpenShift web 控制台 `kubeadmin` 。
1. 按照上面的步骤使用令牌登录到 OpenShift CLI `kubeadmin` `oc login` ，但使用登录到 web 控制台后，请执行这些操作 `kubeadmin` 。
1. 执行以下命令，以启用对 **aad 用户** 的内置注册表的访问。

   ```bash
   # Switch to project "openshift-image-registry"
   oc project openshift-image-registry
   ```

   输出应如下所示。

   ```bash
   Now using project "openshift-image-registry" on server "https://api.x8xl3f4y.eastus.aroapp.io:6443".
   ```

   ```bash
   # Expose the registry using "DefaultRoute"
   oc patch configs.imageregistry.operator.openshift.io/cluster --patch '{"spec":{"defaultRoute":true}}' --type=merge
   ```

   输出应如下所示。

   ```bash
   config.imageregistry.operator.openshift.io/cluster patched
   ```

   ```bash
   # Add roles to "aad-user" for pulling and pushing images
   # Note: replace "<aad-user>" with the one you wrote down before
   oc policy add-role-to-user registry-viewer <aad-user>
   ```

   输出应如下所示。

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-viewer added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

   ```bash
   oc policy add-role-to-user registry-editor <aad-user>
   ```

   输出应如下所示。

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-editor added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

#### <a name="obtain-the-container-registry-url"></a>获取容器注册表 URL

使用 `oc get route` 下图所示的命令获取容器注册表 URL。

```bash
# Note: the value of "Container Registry URL" in the output is the fully qualified registry name.
HOST=$(oc get route default-route --template='{{ .spec.host }}')
echo "Container Registry URL: $HOST"
```

   > [!NOTE]
   > 请注意 **容器注册表 URL** 的控制台输出。 它将用作本指南的完全限定的注册表名称和后续的注册表名称。

## <a name="next-steps"></a>后续步骤

通过在 OpenShift 上部署应用程序来使用内置容器映像注册表。  对于 Java 应用程序，请按照操作方法指南， [使用 Azure Red Hat OpenShift 4 群集上的开放自由/WebSphere 自由部署 java 应用程序](howto-deploy-java-liberty-app.md)。
