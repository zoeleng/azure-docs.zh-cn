---
title: 为目标受众启用功能的分步推出
titleSuffix: Azure App Configuration
description: 了解如何为目标受众启用功能的分步推出
ms.service: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.topic: conceptual
ms.date: 8/7/2020
ms.openlocfilehash: d1574b8a3f8cda3341c0aaf355911e2e93a7bcab
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94557593"
---
# <a name="enable-staged-rollout-of-features-for-targeted-audiences"></a>为目标受众启用功能的分步推出

功能标志允许您在应用程序中动态激活或停用功能。 功能筛选器将在每次计算时确定功能标志的状态。 `Microsoft.FeatureManagement`库包括 `TargetingFilter` ，它为指定的用户和组列表或指定百分比的用户启用功能标志。 `TargetingFilter` 为 "粘滞"。 这意味着，在单个用户收到功能后，他们将继续查看该功能。 你可以使用在 `TargetingFilter` 演示过程中为特定帐户启用一项功能，以便以不同的组或 "振铃" 的方式向用户推出新功能。

在本文中，你将了解如何使用 Azure 应用配置将 ASP.NET Core web 应用程序中的新功能推出给指定的用户和组 `TargetingFilter` 。

## <a name="create-a-web-application-with-feature-flags-and-authentication"></a>创建具有功能标志和身份验证的 web 应用程序

若要根据用户和组推出功能，你将需要一个允许用户登录的 web 应用程序。

1. 使用以下命令创建针对本地数据库进行身份验证的 web 应用程序：

   ```dotnetcli
   dotnet new mvc --auth Individual -o TestFeatureFlags
   ```

1. 生成并运行，然后选择右上角的 " **注册** " 链接以创建新的用户帐户。 使用电子邮件地址 `test@contoso.com` 。 在 " **注册确认** " 屏幕上，选择 " **单击此处确认你的帐户** "。

1. 按照 [快速入门：向 ASP.NET Core 应用添加功能标志](./quickstart-feature-flag-aspnet-core.md) 中的说明将功能标志添加到新的 web 应用程序。

1. 在应用配置中切换功能标志。 验证此操作是否控制导航栏上 **Beta** 项的可见性。

## <a name="update-the-web-application-code-to-use-targetingfilter"></a>更新 web 应用程序代码以使用 TargetingFilter

此时，你可以使用功能标志为所有用户启用或禁用该 `Beta` 功能。 若要为某些用户启用功能标志，而为其他用户禁用该标志，请更新你的代码以使用 `TargetingFilter` 。 在此示例中，将使用登录用户的电子邮件地址作为用户 ID，并使用电子邮件地址的域名部分作为组。 将用户和组添加到 `TargetingContext` 。 `TargetingFilter`使用此上下文来确定每个请求的功能标志的状态。

1. 更新到最新版本的 `Microsoft.FeatureManagement.AspNetCore` 包。

   ```dotnetcli
   dotnet add package Microsoft.FeatureManagement.AspNetCore
   ```

1. 添加 *TestTargetingContextAccessor.cs* 文件：

    ```csharp
    using Microsoft.AspNetCore.Http;
    using Microsoft.FeatureManagement.FeatureFilters;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;

    namespace TestFeatureFlags
    {
        public class TestTargetingContextAccessor : ITargetingContextAccessor
        {
            private const string TargetingContextLookup = "TestTargetingContextAccessor.TargetingContext";
            private readonly IHttpContextAccessor _httpContextAccessor;

            public TestTargetingContextAccessor(IHttpContextAccessor httpContextAccessor)
            {
                _httpContextAccessor = httpContextAccessor ?? throw new ArgumentNullException(nameof(httpContextAccessor));
            }

            public ValueTask<TargetingContext> GetContextAsync()
            {
                HttpContext httpContext = _httpContextAccessor.HttpContext;
                if (httpContext.Items.TryGetValue(TargetingContextLookup, out object value))
                {
                    return new ValueTask<TargetingContext>((TargetingContext)value);
                }
                List<string> groups = new List<string>();
                if (httpContext.User.Identity.Name != null)
                {
                    groups.Add(httpContext.User.Identity.Name.Split("@", StringSplitOptions.None)[1]);
                }
                TargetingContext targetingContext = new TargetingContext
                {
                    UserId = httpContext.User.Identity.Name,
                    Groups = groups
                };
                httpContext.Items[TargetingContextLookup] = targetingContext;
                return new ValueTask<TargetingContext>(targetingContext);
            }
        }
    }
    ```

1. 在 *Startup.cs* 中，添加对 *FeatureFilters* 命名空间的引用：

    ```csharp
    using Microsoft.FeatureManagement.FeatureFilters;
    ```

1. 更新 *ConfigureServices* 方法 `TargetingFilter` ，在调用后进行注册 `AddFeatureManagement()` ：

    ```csharp
    services.AddFeatureManagement()
            .AddFeatureFilter<TargetingFilter>();
    ```

1. 更新 *ConfigureServices* 方法，将 `TestTargetingContextAccessor` 在前面的步骤中创建的添加到服务集合。 每次评估功能标志时， *TargetingFilter* 使用它来确定目标上下文。

    ```csharp
      services.AddSingleton<ITargetingContextAccessor, TestTargetingContextAccessor>();
    ```

整个 *ConfigureServices* 方法将如下所示：

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
            .AddEntityFrameworkStores<ApplicationDbContext>();
    services.AddControllersWithViews();
    services.AddRazorPages();

    // Add feature management, targeting filter, and ITargetingContextAccessor to service collection
    services.AddFeatureManagement().AddFeatureFilter<TargetingFilter>();
    services.AddSingleton<ITargetingContextAccessor, TestTargetingContextAccessor>();
    }
```

## <a name="update-the-feature-flag-to-use-targetingfilter"></a>更新功能标志以使用 TargetingFilter

1. 在 Azure 门户中，请切换到应用配置存储，并选择 " **功能管理器** "。

1. 选择在快速入门中创建的 *Beta* 功能标志的上下文菜单。 选择“编辑”。

    > [!div class="mx-imgBorder"]
    > ![编辑 Beta 功能标志](./media/edit-beta-feature-flag.png)

1. 在 **编辑** 屏幕中，选中 " **启用功能标志** " 复选框（如果尚未选中）。 然后选中 " **使用功能筛选器** " 复选框。

1. 选择 " **目标** " 单选按钮。

1. 选择以下选项：

    - **默认百分比** ：0
    - **组** ：输入 _contoso.com_ 的 **名称** 和 **百分比** _50_
    - **用户** ： `test@contoso.com`

    功能筛选器屏幕将如下所示：

    > [!div class="mx-imgBorder"]
    > ![条件功能标志](./media/feature-flag-filter-enabled.png)

    这些设置会导致以下行为：

    - 始终为用户启用功能标志 `test@contoso.com` ，因为在 `test@contoso.com` " _用户_ " 部分列出了。
    - 已为 _contoso.com_ 组中的其他用户50% 启用了功能标志，因为 _contoso.com_ 在的 " _组_ " 部分中列出，其 _百分比_ 为 _50_ 。
    - 对于所有其他用户，始终禁用该功能，因为 _默认百分比_ 设置为 _0_ 。

1. 选择 " **应用** " 以保存这些设置并返回到 **功能管理器** 屏幕。

1. 功能标志的 **功能筛选器** 现在显示为 " *面向* "。 此状态指示将根据 *目标* 功能筛选器强制执行的条件，按请求启用或禁用功能标志。

## <a name="targetingfilter-in-action"></a>操作中的 TargetingFilter

若要查看此功能标志的效果，请生成并运行该应用程序。 最初， *Beta* 项不会出现在工具栏上，因为 _默认百分比_ 选项设置为0。

现在 `test@contoso.com` 使用注册时设置的密码作为登录。 此 *Beta* 项现在显示在工具栏上，因为 `test@contoso.com` 指定为目标用户。

以下视频显示了正在起作用的这一行为。

> [!div class="mx-imgBorder"]
> ![操作中的 TargetingFilter](./media/feature-flags-targetingfilter.gif)

您可以创建具有 `@contoso.com` 电子邮件地址的其他用户来查看组设置的行为。 50% 的这些用户将看到 *Beta* 项目。 其他50% 看不到 *Beta* 项。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [功能管理概述](./concept-feature-management.md)