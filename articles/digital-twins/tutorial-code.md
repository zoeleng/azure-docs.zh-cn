---
title: 教程： 为客户端应用编写代码
titleSuffix: Azure Digital Twins
description: 使用 .NET (C#) SDK 为客户端应用编写基础代码的教程。
author: baanders
ms.author: baanders
ms.date: 11/02/2020
ms.topic: tutorial
ms.service: digital-twins
ms.openlocfilehash: 3225fff1c82822dee990804f934ada86068841e8
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280262"
---
# <a name="tutorial-coding-with-the-azure-digital-twins-apis"></a>教程：使用 Azure 数字孪生 API 编写代码

开发人员使用 Azure 数字孪生编写客户端应用程序，以与其 Azure 数字孪生服务实例交互，这是很常见的情况。 这篇面向开发人员的教程介绍如何使用[适用于 .NET 的 Azure 数字孪生 SDK (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) 对 Azure 数字孪生服务进行编程。 本教程会逐步引导你从头开始编写 C# 控制台客户端应用。

> [!div class="checklist"]
> * 设置项目
> * 开始使用项目代码   
> * 完整的代码示例
> * 清理资源
> * 后续步骤

## <a name="prerequisites"></a>先决条件

本教程使用命令行执行设置和项目工作。 因此，你可以使用任何代码编辑器来逐步完成练习。

准备工作：
* 任何代码编辑器
* 开发电脑上安装有 .NET Core 3.1。 你可以从[下载 .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) 下载适用于多个平台的此版 .NET Core SDK。

### <a name="prepare-an-azure-digital-twins-instance"></a>准备 Azure 数字孪生实例

[!INCLUDE [Azure Digital Twins: instance prereq](../../includes/digital-twins-prereq-instance.md)]

[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

## <a name="set-up-project"></a>设置项目

准备好使用 Azure 数字孪生实例后，请开始设置客户端应用项目。 

在计算机上打开命令提示符或其他控制台窗口，创建用于存储本教程工作的空项目目录。 将目录命名为任何你喜欢的名称（例如，DigitalTwinsCodeTutorial）。

导航到新目录。

进入项目目录后，创建一个空的 .NET 控制台应用项目。 在命令窗口中运行以下命令，可以为控制台创建基础 C# 项目：

```cmd/sh
dotnet new console
```

这会在你的目录中创建多个文件，其中包括一个名为 Program.cs 的文件，你将在其中编写大部分代码。

让命令窗口保持打开状态，因为整个教程都要继续使用该窗口。

接下来，将两个依赖项添加到你的项目，这是与 Azure 数字孪生结合使用所必需的。 可以使用以下链接导航到 NuGet 上的包，在那里可以找到控制台命令（包括适用于 .NET CLI 的命令）将每个包的最新版本添加到项目中。
* [**Azure.DigitalTwins.Core**](https://www.nuget.org/packages/Azure.DigitalTwins.Core). 这是用于[适用于 .NET 的 Azure 数字孪生 SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) 的包。 
* [**Azure.Identity**](https://www.nuget.org/packages/Azure.Identity). 该库提供可帮助进行 Azure 身份验证的工具。

## <a name="get-started-with-project-code"></a>开始使用项目代码

在本部分中，你将开始编写新应用项目的代码以使用 Azure 数字孪生。 涵盖的操作包括：
* 对服务进行身份验证
* 上传模型
* 捕获错误
* 创建数字孪生
* 创建关系
* 查询数字孪生

在本教程末尾，另有一节显示了完整的代码。 你可以将其用作参考，随时检查自己的程序。

开始前，请在任何代码编辑器中打开 Program.cs 文件。 你将看到如下所示的基础代码模板：

```csharp
using System;

namespace DigitalTwinsCodeTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

首先，在代码的顶部添加一些 `using` 行，以拉取必需的依赖项。

```csharp
using Azure.DigitalTwins.Core;
using Azure.Identity;
```

接下来，需要向此文件添加代码以扩充某些功能。 

### <a name="authenticate-against-the-service"></a>对服务进行身份验证

应用必须执行的第一项操作是对 Azure 数字孪生服务进行身份验证。 接着，你可以创建服务客户端类来访问 SDK 函数。

为了进行身份验证，需要 Azure 数字孪生实例的 hostName。

在 Program.cs 中，将以下代码粘贴到 `Main` 方法中的“Hello, World!”输出行下方。 将 `adtInstanceUrl` 的值设置为 Azure 数字孪生实例 hostName。

```csharp
string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>"; 
var credential = new DefaultAzureCredential();
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
Console.WriteLine($"Service client created – ready to go");
```

保存文件。 

在命令窗口中，使用以下命令运行代码： 

```cmd/sh
dotnet run
```

首次运行时，这会还原依赖项，然后执行程序。 
* 如果未发生错误，程序将输出“Service client created - ready to go”（服务客户端已创建 - 准备就绪）。
* 由于此项目中尚没有任何错误处理机制，因此如果出现任何问题，你将看到代码引发的异常。

### <a name="upload-a-model"></a>上传模型

Azure 数字孪生没有内部域词汇。 环境中可在 Azure 数字孪生中表示的元素类型由你使用模型定义。 [模型](concepts-models.md)类似于面向对象的编程语言中的类；它们为[数字孪生](concepts-twins-graph.md)提供了日后可遵循并实例化的用户定义的模板。 它们以类似于 JSON 的语言编写，这种语言称为数字孪生定义语言 (DTDL)。

创建 Azure 数字孪生解决方案的第一步是在 DTDL 文件中至少定义一个模型。

在创建项目的目录中，创建名为 SampleModel.json 的新 .json 文件 。 粘贴以下文件主体： 

```json
{
  "@id": "dtmi:example:SampleModel;1",
  "@type": "Interface",
  "displayName": "SampleModel",
  "contents": [
    {
      "@type": "Relationship",
      "name": "contains"
    },
    {
      "@type": "Property",
      "name": "data",
      "schema": "string"
    }
  ],
  "@context": "dtmi:dtdl:context;2"
}
```

> [!TIP]
> 如果在本教程中使用 Visual Studio，建议你选择新创建的 JSON 文件，并将属性检查器中的“复制到输出目录”属性设置为“有更新时才复制”或“始终复制”  。 当你在本教程的其余部分使用 F5 执行程序时，这可让 Visual Studio 找到具有默认路径的 JSON 文件。

> [!TIP] 
> 有一种与语言无关的 [DTDL 验证程序示例](/samples/azure-samples/dtdl-validator/dtdl-validator)，可用来检查模型文件，以确保 DTDL 有效。 它是在 DTDL 分析程序库的基础上构建的，详情请参阅 [*操作方法：分析和验证模型*](how-to-parse-models.md)。

接下来，向 Program.cs 添加更多代码，将刚才创建的模型上传到 Azure 数字孪生实例。

首先，在文件顶部添加几个 `using` 语句：

```csharp
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
```

接下来，将 `Main` 方法签名更改为允许异步执行，准备使用 C# 服务 SDK 中的异步方法。 

```csharp
static async Task Main(string[] args)
```

> [!NOTE]
> 因为 SDK 也提供所有调用的同步版本，所以不一定非使用 `async` 不可。 本教程的练习使用 `async`。

接下来是与 Azure 数字孪生服务交互的第一段代码。 此代码将加载你从磁盘创建的 DTDL 文件，然后将其上传到 Azure 数字孪生服务实例。 

将以下代码粘贴到前面添加的授权代码下。

```csharp
Console.WriteLine();
Console.WriteLine($"Upload a model");
var typeList = new List<string>();
string dtdl = File.ReadAllText("SampleModel.json");
typeList.Add(dtdl);
// Upload the model to the service
await client.CreateModelsAsync(typeList);
```

在命令窗口中，使用以下命令运行程序： 

```cmd/sh
dotnet run
```
输出中将打印“Upload a model”，表示已达到此代码，但尚无输出内容指出是否已成功上传。

若要添加一个显示所有模型已成功上传到实例的 print 语句，请在上一段代码后面添加以下代码：

```csharp
// Read a list of models back from the service
Console.WriteLine("Models uploaded to the instance:");
AsyncPageable<DigitalTwinsModelData> modelDataList = client.GetModelsAsync();
await foreach (DigitalTwinsModelData md in modelDataList)
{
    Console.WriteLine($"{md.Id}");
}
```

再次运行程序以测试这段新代码之前，请记得，在上次运行该程序时，已上传了模型。 Azure 数字孪生不允许两次上传相同的模型，因此，如果尝试再次上传相同的模型，则程序会引发异常。

考虑到这一点，请在命令窗口中，再次使用以下命令运行程序：

```cmd/sh
dotnet run
```

程序应该会引发异常。 当你尝试上传已上传过的模型时，服务会通过 REST API 返回“请求错误”错误。 因此，Azure 数字孪生客户端 SDK 会针对不成功的每个服务返回代码引发异常。 

下一节将讨论与此类似的异常以及如何在代码中予以处理。

### <a name="catch-errors"></a>捕获错误

为了防止程序崩溃，可以在模型上传代码周围添加异常代码。 在 try/catch 处理程序中包装现有的客户端调用 `await client.CreateModelsAsync(typeList)`，如下所示：

```csharp
try {
    await client.CreateModelsAsync(typeList);
} catch (RequestFailedException rex) {
    Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
}
```

现在，如果在命令窗口中使用 `dotnet run` 运行程序，则会看到返回的错误代码。 模型创建代码的输出显示此错误：

:::image type="content" source= "media/tutorial-code/model-error.png" alt-text="程序输出，其中显示一条消息，指出“409: 服务请求失败。状态：409 (冲突)。”，后跟一个指示 dtmi:example:SampleModel;1 已存在的错误输出":::

从此时起，本教程会将所有对服务方法的调用包装在 try/catch 处理程序中。

### <a name="create-digital-twins"></a>创建数字孪生

将模型上传到 Azure 数字孪生后，可以使用此模型定义创建数字孪生。 [数字孪生](concepts-twins-graph.md)是模型的实例，表示业务环境中的实体，例如农场中的传感器、大楼中的房间或汽车上的灯。 本部分将在你之前上传的模型的基础上创建几个数字孪生。

将以下代码添加到 `Main` 方法的末尾，以根据此模型创建和初始化三个数字孪生体。

```csharp
// Initialize twin data
BasicDigitalTwin twinData = new BasicDigitalTwin();
twinData.Metadata.ModelId = "dtmi:example:SampleModel;1";
twinData.Contents.Add("data", $"Hello World!");

string prefix="sampleTwin-";
for(int i=0; i<3; i++) {
    try {
        twinData.Id = $"{prefix}{i}";
        await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twinData.Id, twinData);
        Console.WriteLine($"Created twin: {prefix}{i}");
    } catch(RequestFailedException rex) {
        Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
    }
}
```

在命令窗口中，使用 `dotnet run` 运行程序。 在输出中，查找指示已创建 sampleTwin-0、sampleTwin-1 和 sampleTwin-2 的打印消息  。 

然后，再次运行程序。 

请注意，第二次创建孪生时，即使孪生在第一次运行后已经存在，也不会引发任何错误。 与创建模型不同，在 REST 级别上，创建孪生是 PUT 调用加上 upsert 语义 。 这意味着，如果已存在一个孪生体，则再次尝试创建相同的孪生体只会取代原来的孪生体。 不会引发错误。

### <a name="create-relationships"></a>创建关系

接下来，你可以在已创建的孪生之间创建关系，将它们连接到孪生图 。 [孪生图](concepts-twins-graph.md)用于表示整个环境。

将新的静态方法添加到 `Main` 方法下的 `Program` 类（代码现在具有两种方法）：

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
{
    var relationship = new BasicRelationship
    {
        TargetId = targetId,
        Name = "contains"
    };

    try
    {
        string relId = $"{srcId}-contains->{targetId}";
        await client.CreateOrReplaceRelationshipAsync(srcId, relId, relationship);
        Console.WriteLine("Created relationship successfully");
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
    }
}
```

接下来，将以下代码添加到 `Main` 方法的末尾，以调用 `CreateRelationship` 方法并使用刚刚编写的代码：

```csharp
// Connect the twins with relationships
await CreateRelationship(client, "sampleTwin-0", "sampleTwin-1");
await CreateRelationship(client, "sampleTwin-0", "sampleTwin-2");
```

在命令窗口中，使用 `dotnet run` 运行程序。 在输出中，查找指出这两种关系已成功创建的 print 语句。

请注意，如果已存在另一个 ID 相同的关系，Azure 数字孪生将不会让你创建关系；因此，如果多次运行程序，你将会看到有关创建关系的异常信息。 此代码会捕获异常并将其忽略。 

### <a name="list-relationships"></a>列出关系

你将添加的下一段代码可让你查看已创建的关系列表。

将以下新方法添加到 `Program` 类：

```csharp
public async static Task ListRelationships(DigitalTwinsClient client, string srcId)
{
    try {
        AsyncPageable<BasicRelationship> results = client.GetRelationshipsAsync<BasicRelationship>(srcId);
        Console.WriteLine($"Twin {srcId} is connected to:");
        await foreach (BasicRelationship rel in results)
        {
            Console.WriteLine($" -{rel.Name}->{rel.TargetId}");
        }
    } catch (RequestFailedException rex) {
        Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
    }
}
```

然后，将以下代码添加到 `Main` 方法的末尾，以调用 `ListRelationships` 代码：

```csharp
//List the relationships
await ListRelationships(client, "sampleTwin-0");
```

在命令窗口中，使用 `dotnet run` 运行程序。 应该会在 output 语句中看到已创建的所有关系的列表，如下所示：

:::image type="content" source= "media/tutorial-code/list-relationships.png" alt-text="程序输出，其中显示一条消息，指出“孪生体 sampleTwin-0 已连接到: contains->sampleTwin-1、-contains->sampleTwin-2”":::

### <a name="query-digital-twins"></a>查询数字孪生

Azure 数字孪生的主要功能是能够轻松有效地[查询](concepts-query-language.md)孪生图，以解答有关环境的问题。

要在本教程中添加的最后一段代码会对对 Azure 数字孪生实例运行查询。 本示例中使用的查询会返回此实例中的所有数字孪生。

添加此 `using` 语句，以允许使用 `JsonSerializer` 类来帮助显示数字孪生体信息：

```csharp
using System.Text.Json;
```

然后，在 `Main` 方法的末尾添加以下代码：

```csharp
// Run a query for all twins   
string query = "SELECT * FROM digitaltwins";
AsyncPageable<BasicDigitalTwin> result = client.QueryAsync<BasicDigitalTwin>(query);

await foreach (BasicDigitalTwin twin in result)
{
    Console.WriteLine(JsonSerializer.Serialize(twin));
    Console.WriteLine("---------------");
}
```

在命令窗口中，使用 `dotnet run` 运行程序。 你应该会在输出中看到此实例中的所有数字孪生。

## <a name="complete-code-example"></a>完整代码示例

到本教程的此阶段，你已有一个完整的客户端应用，能够对 Azure 数字孪生执行基本操作。 下面列出了 Program.cs 中的程序的完整代码供你参考：

```csharp
using System;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
using System.Text.Json;

namespace minimal
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("Hello World!");
            
            string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>"; 
            
            var credential = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
            Console.WriteLine($"Service client created – ready to go");

            Console.WriteLine();
            Console.WriteLine($"Upload a model");
            var typeList = new List<string>();
            string dtdl = File.ReadAllText("SampleModel.json");
            typeList.Add(dtdl);

            // Upload the model to the service
            try {
                await client.CreateModelsAsync(typeList);
            } catch (RequestFailedException rex) {
                Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
            }
            // Read a list of models back from the service
            Console.WriteLine("Models uploaded to the instance:");
            AsyncPageable<DigitalTwinsModelData> modelDataList = client.GetModelsAsync();
            await foreach (DigitalTwinsModelData md in modelDataList)
            {
                Console.WriteLine($"{md.Id}");
            }

            // Initialize twin data
            BasicDigitalTwin twinData = new BasicDigitalTwin();
            twinData.Metadata.ModelId = "dtmi:example:SampleModel;1";
            twinData.Contents.Add("data", $"Hello World!");
            
            string prefix="sampleTwin-";
            for(int i=0; i<3; i++) {
                try {
                    twinData.Id = $"{prefix}{i}";
                    await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twinData.Id, twinData);
                    Console.WriteLine($"Created twin: {prefix}{i}");
                } catch(RequestFailedException rex) {
                    Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
                }
            }

            // Connect the twins with relationships
            await CreateRelationship(client, "sampleTwin-0", "sampleTwin-1");
            await CreateRelationship(client, "sampleTwin-0", "sampleTwin-2");

            //List the relationships
            await ListRelationships(client, "sampleTwin-0");

            // Run a query for all twins   
            string query = "SELECT * FROM digitaltwins";
            AsyncPageable<BasicDigitalTwin> result = client.QueryAsync<BasicDigitalTwin>(query);
            
            await foreach (BasicDigitalTwin twin in result)
            {
                Console.WriteLine(JsonSerializer.Serialize(twin));
                Console.WriteLine("---------------");
            }
        }

        public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
        {
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = "contains"
            };
        
            try
            {
                string relId = $"{srcId}-contains->{targetId}";
                await client.CreateOrReplaceRelationshipAsync(srcId, relId, relationship);
                Console.WriteLine("Created relationship successfully");
            }
            catch (RequestFailedException rex) {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }
        }
        
        public async static Task ListRelationships(DigitalTwinsClient client, string srcId)
        {
            try {
                AsyncPageable<BasicRelationship> results = client.GetRelationshipsAsync<BasicRelationship>(srcId);
                Console.WriteLine($"Twin {srcId} is connected to:");
                await foreach (BasicRelationship rel in results)
                {
                    Console.WriteLine($" -{rel.Name}->{rel.TargetId}");
                }
            } catch (RequestFailedException rex) {
                Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
            }
        }

    }
}
```
## <a name="clean-up-resources"></a>清理资源
 
本教程中使用的实例可重复用于下一教程 [*教程：使用示例客户端应用了解基础知识*](tutorial-command-line-app.md)。 如果打算继续学习下一个教程，可以保留在此处设置的 Azure 数字孪生实例。
 
[!INCLUDE [digital-twins-cleanup-basic.md](../../includes/digital-twins-cleanup-basic.md)]

最后，删除在本地计算机上创建的项目文件夹。

## <a name="next-steps"></a>后续步骤

在本教程中，你从头开始创建了 .NET 控制台客户端应用程序。 你为此客户端应用编写了代码，在 Azure 数字孪生实例上执行基本操作。

请继续学习下一个教程，了解可使用这类示例客户端应用执行的操作： 

> [!div class="nextstepaction"]
> [*教程：使用示例客户端应用了解基础知识*](tutorial-command-line-app.md)
