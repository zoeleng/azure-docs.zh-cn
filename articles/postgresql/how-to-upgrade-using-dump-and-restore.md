---
title: 使用转储和还原 Azure Database for PostgreSQL 升级-单服务器
description: 介绍几种用于转储和还原数据库以迁移到 Azure Database for PostgreSQL-单服务器版本更高的版本的方法。
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 11/05/2020
ms.openlocfilehash: 6dfcf0b2ec1d46821007123908a8e7ba8df29744
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421758"
---
# <a name="upgrade-your-postgresql-database-using-dump-and-restore"></a>使用转储和还原升级 PostgreSQL 数据库

在 Azure Database for PostgreSQL 单服务器中，建议使用以下方法之一将 PostgreSQL 数据库引擎升级到更高的主要版本：
* 使用 PostgreSQL [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) 和 [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html)的脱机方法。 在此方法中，首先从源服务器执行转储，然后在目标服务器中还原该转储。
* 使用 [**数据库迁移服务**](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal) (dm) 的联机方法。 此方法使目标数据库与源保持同步，并且可以选择何时剪切。 但是，需要解决一些先决条件和限制。 

在联机和脱机方法之间进行决定以执行主要版本升级时，可以使用以下建议。

| **数据库** | **转储/还原 (脱机)** | **DM (Online)** |
| ------ | :------: | :-----: |
| 你有一个小型数据库，并可以承受升级的停机时间  | X | |
| 小型数据库 ( # A0 10 GB)   | X | X | 
| 中小型 Db (10 GB – 100 GB)  | X | X |
| 大型数据库 ( # A0 100 GB)  |  | X |
| 无论数据库大小如何) ，都可以承受 (升级的停机时间。 | X |  |
| 能否解决 DM [先决条件](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal#prerequisites) ，包括重启？ |  | X |
| 在升级过程中，可以避免 Ddl 和无日志记录表？ | |  X |

本操作方法指南提供了两个示例方法，说明如何使用 PostgreSQL pg_dump 和 pg_restore 命令升级数据库。 尽管将数据库从源服务器 **迁移** 到目标服务器，但本文档中的过程称为 " **升级** "。 

> [!NOTE]
> 可以通过多种方式执行 PostgreSQL 转储和还原。 您可以选择使用与本文档中所述方法不同的方法。 例如，若要在 PostgreSQL 客户端中执行转储，请参阅 [文档](./howto-migrate-using-dump-and-restore.md) ，了解分步过程的详细信息和最佳实践。 有关其他参数的详细转储和还原语法，请参阅文章 [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) 和 [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html)。 


## <a name="prerequisites-for-using-dump-and-restore-with-azure-postgresql"></a>在 Azure PostgreSQL 中使用转储和还原的先决条件
 
若要逐步执行本操作指南，你需要：
- 运行9.5、9.6 或 10 (Azure Database for PostgreSQL –单服务器) 的源数据库
- 目标数据库服务器，具有所需的 PostgreSQL 主版本 [Azure Database for PostgreSQL 服务器](quickstart-create-server-database-portal.md)。 
- 安装了 PostgreSQL 的客户端系统 (Linux) 并且安装了 [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) 和 [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) 命令行实用程序。 
- 或者，您可以使用 [Azure Cloud Shell](https://shell.azure.com) 或单击 [Azure 门户](https://portal.azure.com)右上角菜单栏上的 Azure Cloud Shell。 在 `az login` 运行转储和还原命令之前，你必须登录到你的帐户。
- 与源服务器和目标服务器) 在同一区域中运行的 PostgreSQL 客户端位置（如 VM）。 

## <a name="additional-details-and-considerations"></a>其他详细信息和注意事项
- 通过单击门户中的 "连接字符串"，可以找到源数据库和目标数据库的连接字符串。 
- 您可能正在服务器上运行多个数据库。 你可以通过连接到你的源服务器并运行来查找数据库列表 `\l` 。
- 在目标数据库服务器中创建相应的数据库。
- 您可以跳过升级 `azure_maintenance` 或模板数据库。
- 请参阅上表，确定数据库是否适用于这种迁移模式。
- 如果要使用 Azure Cloud Shell，则会话将在20分钟后超时。 如果数据库大小 < 10 GB，则可能会在不超时的情况下完成升级。否则，你可能必须通过其他方式（例如 <Enter> 在10-15 分钟内按一次键）来打开会话。 

> [!TIP] 
> - 如果为源数据库和目标数据库使用相同的密码，则可以设置 `PGPASSWORD=yourPassword` 环境变量。  然后，每次运行命令（如 psql、pg_dump 和 pg_restore）都无需提供密码。  同样，可以设置其他变量，如等 `PGUSER` `PGSSLMODE` 。请参阅 [PostgreSQL 环境变量](https://www.postgresql.org/docs/11/libpq-envars.html)。
>  
> - 如果 PostgreSQL 服务器需要 TLS/SSL 连接（默认情况下在 Azure Database for PostgreSQL 服务器中启用），请设置环境变量 `PGSSLMODE=require`，以便 pg_restore 工具使用 TLS 进行连接。 如果不使用 TLS，错误可能会显示为 `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> - 在 Windows 命令行中，在运行 pg_restore 命令之前运行命令 `SET PGSSLMODE=require`。 在 Linux 或 Bash 中，在运行 pg_restore 命令之前运行命令 `export PGSSLMODE=require`。

## <a name="example-database-used-in-this-guide"></a>本指南中使用的示例数据库

 | **说明** | **值** |
 | ------- | ------- |
 | 源服务器 (v 9.5)  | pg-95.postgres.database.azure.com |
 | 源数据库 | bench5gb |
 | 源数据库大小 | 5 GB |
 | 源用户名 | pg@pg-95 |
 | 目标服务器 (v11)  | pg-11.postgres.database.azure.com |
 | 目标数据库 | bench5gb |
 | 目标用户名 | pg@pg-11 |

## <a name="method-1-upgrade-with-streaming-backups-to-the-target"></a>方法1：将流式备份升级到目标 

在此方法中，整个数据库转储会直接流式传输到目标数据库服务器，并且不会在客户端中存储转储。 因此，这可用于具有有限存储的客户端，甚至可以从 Azure Cloud Shell 运行。 

1. 请确保使用命令在目标服务器上存在该数据库 `\l` 。 如果数据库不存在，则创建数据库。
   ```azurecli-interactive
    psql "host=myTargetServer port=5432 dbname=postgres user=myUser password=###### sslmode=mySSLmode"
    ```
    ```SQL
    postgres> \l   
    postgres> create database myTargetDB;
   ```

2. 使用管道运行转储并还原为单个命令行。 
    ```azurecli-interactive
    pg_dump -Fc -v --mySourceServer --port=5432 --username=myUser --dbname=mySourceDB | pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB
    ```

    例如，

    ```azurecli-interactive
    pg_dump -Fc -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb | pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb
    ```  
3. 升级 (迁移) 过程完成后，便可以通过目标服务器测试应用程序。 
4. 为服务器中的所有数据库重复此过程。

 下表说明了使用此方法升级所需的时间。 使用 [pgbench](https://www.postgresql.org/docs/10/pgbench.html)填充数据。 由于你的数据库可以具有不同于 pgbench 生成的表和索引的不同大小的对象，因此强烈建议测试数据库的转储和还原，以了解升级数据库所需的实际时间。 

| **数据库大小** | **大约时间** | 
| ----- | ------ |
| 1 GB  | 1-2 分钟 |
| 5 GB | 8-10 分钟 |
| 10 GB | 15-20 分钟 |
| 50 GB | 1-1.5 小时 |
| 100 GB | 2.5-3 小时|
   
## <a name="method-2-upgrade-with-parallel-dump-and-restore"></a>方法2：通过并行转储和还原进行升级 

如果数据库中有较大的表，并且想要对该数据库的转储和还原过程进行并行化，则此方法非常有用。 需要足够的本地磁盘存储来容纳数据库的备份转储。 这种并行转储和还原过程减少了完成整个迁移/升级所用的时间。 例如，在不到30分钟的时间内完成需要1到1.5 小时的迁移的 50 GB pgbench 数据库。

1. 对于源服务器中的每个数据库，在目标服务器上创建相应的数据库。

   ```bash
    psql "host=myTargetServer port=5432 dbname=postgres user=myuser password=###### sslmode=mySSLmode"
    postgresl> create database myDB;
   ```
   例如，
    ```bash
    psql "host=pg-11.postgres.database.azure.com port=5432 dbname=postgres user=pg@pg-11 password=###### sslmode=require"

    postgres> create database bench5gb;
    postgres> \q
    ```

2. 以目录格式运行 pg_dump 命令，该格式的作业数 = 4 (数据库) 中的表数。 对于更大的计算层和多个表，你可以将其增加到更大的数字。 该 pg_dump 将创建一个目录，用于存储每个作业的压缩文件。

    ```bash
    pg_dump -Fd -v --host=sourceServer --port=5432 --username=myUser --dbname=mySourceDB -j 4 -f myDumpDirectory
    ```
    例如，
    ```bash
    pg_dump -Fd -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb -j 4 -f dump.dir
    ```

3. 然后在目标服务器上还原备份。
    ```bash
    $ pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB -j 4 myDumpDir
    ```
    例如，
    ```bash
    $ pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb -j 4 dump.dir
    ```

> [!TIP]
> 本文档中所述的过程还可用于升级 Azure Database for PostgreSQL 灵活的服务器，该服务器处于预览阶段。 主要区别是灵活服务器目标的连接字符串不包含 `@dbName` 。  例如，如果用户名为 `pg` ，则连接字符串中的单个服务器的用户名将为 `pg@pg-95` ，而使用灵活的服务器时，只需使用即可 `pg` 。

## <a name="next-steps"></a>后续步骤

- 如果对目标数据库函数满意，则可以删除旧的数据库服务器。 
- 如果要使用与源服务器相同的数据库终结点，则在删除旧的源数据库服务器后，可以使用旧的数据库服务器名称创建读取副本。 确定稳定状态后，可以停止副本，这会将副本服务器提升为独立服务器。 有关更多详细信息，请参阅[复制](./concepts-read-replicas.md)。
- 请记住先在测试环境中测试和验证这些命令，然后再将其用于生产。
