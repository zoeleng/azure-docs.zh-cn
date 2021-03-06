---
title: 使用 Azure 导入/导出将数据传输到 Azure Blob | Microsoft Docs
description: 了解如何在 Azure 门户中创建导入和导出作业，以便将数据传入和传出 Azure Blob。
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 10/29/2020
ms.author: alkohli
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d23560e8ee387ca8bc9cb4bba4211f6c8272addd
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94490876"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>使用 Azure 导入/导出服务将数据导入到 Azure Blob 存储

本文提供了有关如何使用 Azure 导入/导出服务安全地将大量数据导入到 Azure Blob 存储的分步说明。 若要将数据导入到 Azure Blob，此服务要求你将包含数据的已加密磁盘驱动器寄送到某个 Azure 数据中心。

## <a name="prerequisites"></a>先决条件

在创建导入作业来将数据传输到 Azure Blob 存储之前，请仔细查看并完成此服务的以下先决条件列表。
必须具备以下条件：

* 拥有可用于导入/导出服务的有效 Azure 订阅。
* 拥有至少一个包含存储容器的 Azure 存储帐户。 请参阅[导入/导出服务支持的存储帐户和存储类型](storage-import-export-requirements.md)的列表。
  * 有关创建新存储帐户的信息，请参阅[如何创建存储帐户](storage-account-create.md)。
  * 有关存储容器的信息，请转到[创建存储容器](../blobs/storage-quickstart-blobs-portal.md#create-a-container)。
* 拥有足够数量的[受支持类型](storage-import-export-requirements.md#supported-disks)的磁盘。
* 拥有运行[受支持 OS 版本](storage-import-export-requirements.md#supported-operating-systems)的 Windows 系统。
* 在 Windows 系统上启用 BitLocker。 请参阅[如何启用 BitLocker](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/)。
* 在 Windows 系统上[下载最新的 WAImportExport 版本 1](https://www.microsoft.com/download/details.aspx?id=42659)。 该工具的最新版本包含安全更新，允许对 BitLocker 密钥使用外部保护程序并更新了解锁模式功能。

  * 解压缩到默认文件夹 `waimportexportv1`。 例如，`C:\WaImportExportV1`。
* 具有 FedEx/DHL 帐户。 如果要使用 FedEx/DHL 以外的运营商，请联系 Azure Data Box 运营团队 `adbops@microsoft.com` 。
  * 该帐户必须是有余额的有效帐户，且有退货功能。
  * 生成导出作业的跟踪号。
  * 每个作业都应有一个单独的跟踪号。 不支持多个作业共享相同跟踪号。
  * 如果没有承运商帐户，请转到：
    * [创建 FedEx 帐户](https://www.fedex.com/en-us/create-account.html)，或
    * [创建 DHL 帐户](http://www.dhl-usa.com/en/express/shipping/open_account.html)。

## <a name="step-1-prepare-the-drives"></a>步骤 1：准备驱动器

此步骤生成一个日志文件。 日志文件存储着驱动器序列号、加密密钥和存储帐户详细信息等基本信息。

请执行以下步骤来准备驱动器。

1. 通过 SATA 连接器将磁盘驱动器连接到 Windows 系统。
2. 在每个驱动器上创建一个 NTFS 卷。 为卷分配驱动器号。 不要使用装入点。
3. 在 NTFS 卷上启用 BitLocker 加密。 如果使用某个 Windows Server 系统，请使用[如何在 Windows Server 2012 R2 上启用 BitLocker](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/) 中的说明。
4. 将数据复制到加密的卷。 可使用拖放或 Robocopy 或任何类似的复制工具。 在运行该工具的同一文件夹中会创建一个日志 (.jrn) 文件。

   如果驱动器已锁定并且需要解锁，不同用例的解锁步骤可能不同。

   * 如果已将数据添加到预加密驱动器（并非使用 WAImportExport 工具来加密），请使用弹出窗口中的 BitLocker 密钥（你指定的数字密码）来解锁驱动器。

   * 如果已将数据添加到由 WAImportExport 工具加密的驱动器，请使用以下命令解锁该驱动器：

        `WAImportExport Unlock /externalKey:<BitLocker key (base 64 string) copied from journal (*.jrn*) file>`

5. 使用管理权限打开 PowerShell 或命令行窗口。 若要将目录切换到解压缩的文件夹，请运行以下命令：

    `cd C:\WaImportExportV1`
6. 若要获取驱动器的 BitLocker 密钥，请运行以下命令：

    `manage-bde -protectors -get <DriveLetter>:`
7. 若要准备磁盘，请运行以下命令。 **这可能要花费几小时到几天时间，具体取决于数据大小。**

    ```powershell
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session<session number> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /blobtype:<BlockBlob or PageBlob> /skipwrite
    ```

    在运行该工具的同一文件夹中会创建一个日志文件。 还会创建两个其他文件 - 一个 *.xml* 文件（您在其中运行工具的文件夹）和一个 *drive-manifest.xml* 文件（数据所在的文件夹）。

    下表介绍了所使用的参数：

    |选项  |说明  |
    |---------|---------|
    |/j:     |带有 .jrn 扩展名的日志文件的名称。 会为每个驱动器生成一个日志文件。 建议使用磁盘序列号作为日志文件名。         |
    |/id:     |会话 ID。 请为该命令的每个实例使用唯一的会话编号。      |
    |/t:     |要寄送的磁盘的驱动器号。 例如，驱动器 `D`。         |
    |/bk:     |驱动器的 BitLocker 密钥。 其数字密码来自 `manage-bde -protectors -get D:` 的输出      |
    |/srcdir:     |要寄送的磁盘的驱动器号后跟 `:\`。 例如，`D:\`。         |
    |/dstdir:     |Azure 存储中的目标容器的名称。         |
    |/blobtype:     |此选项指定要将数据导入到的 Blob 的类型。 对于块 blob，此项为 `BlockBlob`；对于页 blob，此项为 `PageBlob`。         |
    |/skipwrite:     |此选项指定没有需要复制的新数据并且要准备磁盘上的现有数据。          |
    |/enablecontentmd5:     |启用此选项时，将确保计算 MD5 并将其设置为每个 blob 上的 `Content-md5` 属性。 仅当希望在将数据上传到 Azure 后使用 `Content-md5` 字段时，才使用此选项。 <br> 此选项不影响数据完整性检查（默认情况下会进行）。 此设置确实会增加将数据上传到云所需的时间。          |
8. 为需要寄送的每个磁盘重复前面的步骤。 每次运行该命令行时，都会使用所提供的名称创建一个日志文件。

    > [!IMPORTANT]
    > * 与日志文件一起，还会在工具所在的同一文件夹中创建一个 `<Journal file name>_DriveInfo_<Drive serial ID>.xml` 文件。 如果日志文件太大，在创建作业时会使用该 .xml 文件而不使用日志文件。

## <a name="step-2-create-an-import-job"></a>步骤 2：创建导入作业

### <a name="portal"></a>[门户](#tab/azure-portal)

在 Azure 门户中执行以下步骤来创建导入作业。

1. 登录到 https://portal.azure.com/ 。
2. 转到“所有服务”>“存储”>“导入/导出作业”。

    ![转到导入/导出作业](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. 单击“创建导入/导出作业”。

    ![单击“创建导入/导出作业”](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. 在“基本信息”中：

   * 选择“导入到 Azure”。
   * 输入导入作业的描述性名称。 可使用此名称来跟踪作业进度。
       * 此名称只能包含小写字母、数字和连字符。
       * 此名称必须以字母开头，并且不得包含空格。
   * 选择一个订阅。
   * 输入或选择一个资源组。

     ![创建导入作业 - 步骤 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

5. 在“作业详细信息”中：

   * 上传你在驱动器准备步骤中获取的驱动器日志文件。 如果使用了 `waimportexport.exe version1`，请为你准备的每个驱动器上传一个文件。 如果日志文件大小超过了 2 MB，则可以使用随日志文件创建的 `<Journal file name>_DriveInfo_<Drive serial ID>.xml`。
   * 选择将用来存放数据的目标存储帐户。
   * 放置位置会根据选定存储帐户所属的区域自动进行填充。

   ![创建导入作业 - 步骤 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

6. 在“回寄信息”中：

   * 从下拉列表中选择承运商。 如果要使用 FedEx/DHL 以外的电信公司，请从下拉列表中选择现有的选项。 请与 Azure Data Box 运营团队联系， `adbops@microsoft.com`  并提供有关计划使用的电信公司的信息。
   * 输入你已在该承运商那里创建的有效承运商帐户编号。 导入作业完成后，Microsoft 使用此帐户寄回驱动器。 如果还没有帐户编号，请创建一个 [FedEx](https://www.fedex.com/us/oadr/) 或 [DHL](https://www.dhl.com/) 承运商帐户。
   * 提供完整、有效的联系人姓名、电话号码、电子邮件地址、街道地址、城市、邮政编码、省/自治区/直辖市和国家/地区。

       > [!TIP]
       > 请提供组电子邮件，而非为单个用户指定电子邮件地址。 这可确保即使管理员离开也会收到通知。

     ![创建导入作业 - 步骤 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

7. 在“摘要”中：

   * 在摘要中复查提供的作业信息。 记下作业名称和 Azure 数据中心送货地址，以便将将磁盘寄回 Azure。 稍后将在发货标签中使用此信息。
   * 单击“确定”以创建导入作业。

     ![创建导入作业 - 步骤 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

使用以下步骤在 Azure CLI 中创建导入作业。

[!INCLUDE [azure-cli-prepare-your-environment-h3.md](../../../includes/azure-cli-prepare-your-environment-h3.md)]

### <a name="create-a-job"></a>创建作业

1. 使用 [az extension add](/cli/azure/extension#az_extension_add) 命令添加 [az import-export](/cli/azure/ext/import-export/import-export) extension：

    ```azurecli
    az extension add --name import-export
    ```

1. 可以使用现有资源组，也可以创建一个。 若要创建资源组，请运行 [az group create](/cli/azure/group#az_group_create) 命令：

    ```azurecli
    az group create --name myierg --location "West US"
    ```

1. 可以使用现有存储帐户，也可以创建一个存储帐户。 若要创建存储帐户，请运行 [az storage account create](/cli/azure/storage/account#az_storage_account_create) 命令：

    ```azurecli
    az storage account create --resource-group myierg --name myssdocsstorage --https-only
    ```

1. 若要获取可将磁盘寄送到的位置列表，请使用 [az import-export location list](/cli/azure/ext/import-export/import-export/location#ext_import_export_az_import_export_location_list) 命令：

    ```azurecli
    az import-export location list
    ```

1. 使用 [az import-export location show](/cli/azure/ext/import-export/import-export/location#ext_import_export_az_import_export_location_show) 命令获取你所在地区的位置：

    ```azurecli
    az import-export location show --location "West US"
    ```

1. 运行以下 [az import-export create](/cli/azure/ext/import-export/import-export#ext_import_export_az_import_export_create) 命令来创建导入作业：

    ```azurecli
    az import-export create \
        --resource-group myierg \
        --name MyIEjob1 \
        --location "West US" \
        --backup-drive-manifest true \
        --diagnostics-path waimportexport \
        --drive-list bit-locker-key=439675-460165-128202-905124-487224-524332-851649-442187 \
            drive-header-hash= drive-id=AZ31BGB1 manifest-file=\\DriveManifest.xml \
            manifest-hash=69512026C1E8D4401816A2E5B8D7420D \
        --type Import \
        --log-level Verbose \
        --shipping-information recipient-name="Microsoft Azure Import/Export Service" \
            street-address1="3020 Coronado" city="Santa Clara" state-or-province=CA postal-code=98054 \
            country-or-region=USA phone=4083527600 \
        --return-address recipient-name="Gus Poland" street-address1="1020 Enterprise way" \
            city=Sunnyvale country-or-region=USA state-or-province=CA postal-code=94089 \
            email=gus@contoso.com phone=4085555555" \
        --return-shipping carrier-name=FedEx carrier-account-number=123456789 \
        --storage-account myssdocsstorage
    ```

   > [!TIP]
   > 请提供组电子邮件，而非为单个用户指定电子邮件地址。 这可确保即使管理员离开也会收到通知。

1. 使用 [az import-export list](/cli/azure/ext/import-export/import-export#ext_import_export_az_import_export_list) 命令查看 myierg 资源组的所有作业：

    ```azurecli
    az import-export list --resource-group myierg
    ```

1. 若要更新作业或取消作业，请运行 [az import-export update](/cli/azure/ext/import-export/import-export#ext_import_export_az_import_export_update) 命令：

    ```azurecli
    az import-export update --resource-group myierg --name MyIEjob1 --cancel-requested true
    ```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

使用以下步骤在 Azure PowerShell 中创建导入作业。

[!INCLUDE [azure-powershell-requirements-h3.md](../../../includes/azure-powershell-requirements-h3.md)]

> [!IMPORTANT]
> **ImportExport** PowerShell 模块为预览版时，必须使用 cmdlet 单独安装它 `Install-Module` 。 此 PowerShell 模块正式发布后，它会包含在将来的 Az PowerShell 模块发行版中，并在 Azure Cloud Shell 中默认提供。

```azurepowershell-interactive
Install-Module -Name Az.ImportExport
```

### <a name="create-a-job"></a>创建作业

1. 可以使用现有资源组，也可以创建一个。 若要创建资源组，请运行 [AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet：

   ```azurepowershell-interactive
   New-AzResourceGroup -Name myierg -Location westus
   ```

1. 可以使用现有存储帐户，也可以创建一个存储帐户。 若要创建存储帐户，请运行 [AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) cmdlet：

   ```azurepowershell-interactive
   New-AzStorageAccount -ResourceGroupName myierg -AccountName myssdocsstorage -SkuName Standard_RAGRS -Location westus -EnableHttpsTrafficOnly $true
   ```

1. 若要获取可将磁盘寄送到的位置列表，请使用 [AzImportExportLocation](/powershell/module/az.importexport/get-azimportexportlocation) cmdlet：

   ```azurepowershell-interactive
   Get-AzImportExportLocation
   ```

1. 将 `Get-AzImportExportLocation` cmdlet 与参数配合使用 `Name` 可获取区域的位置：

   ```azurepowershell-interactive
   Get-AzImportExportLocation -Name westus
   ```

1. 运行以下 [AzImportExport](/powershell/module/az.importexport/new-azimportexport) 示例来创建导入作业：

   ```azurepowershell-interactive
   $driveList = @(@{
     DriveId = '9CA995BA'
     BitLockerKey = '439675-460165-128202-905124-487224-524332-851649-442187'
     ManifestFile = '\\DriveManifest.xml'
     ManifestHash = '69512026C1E8D4401816A2E5B8D7420D'
     DriveHeaderHash = 'AZ31BGB1'
   })

   $Params = @{
      ResourceGroupName = 'myierg'
      Name = 'MyIEjob1'
      Location = 'westus'
      BackupDriveManifest = $true
      DiagnosticsPath = 'waimportexport'
      DriveList = $driveList
      JobType = 'Import'
      LogLevel = 'Verbose'
      ShippingInformationRecipientName = 'Microsoft Azure Import/Export Service'
      ShippingInformationStreetAddress1 = '3020 Coronado'
      ShippingInformationCity = 'Santa Clara'
      ShippingInformationStateOrProvince = 'CA'
      ShippingInformationPostalCode = '98054'
      ShippingInformationCountryOrRegion = 'USA'
      ShippingInformationPhone = '4083527600'
      ReturnAddressRecipientName = 'Gus Poland'
      ReturnAddressStreetAddress1 = '1020 Enterprise way'
      ReturnAddressCity = 'Sunnyvale'
      ReturnAddressStateOrProvince = 'CA'
      ReturnAddressPostalCode = '94089'
      ReturnAddressCountryOrRegion = 'USA'
      ReturnAddressPhone = '4085555555'
      ReturnAddressEmail = 'gus@contoso.com'
      ReturnShippingCarrierName = 'FedEx'
      ReturnShippingCarrierAccountNumber = '123456789'
      StorageAccountId = '/subscriptions/<SubscriptionId>/resourceGroups/myierg/providers/Microsoft.Storage/storageAccounts/myssdocsstorage'
   }
   New-AzImportExport @Params
   ```

   > [!TIP]
   > 请提供组电子邮件，而非为单个用户指定电子邮件地址。 这可确保即使管理员离开也会收到通知。

1. 使用 [AzImportExport](/powershell/module/az.importexport/get-azimportexport) cmdlet 查看 myierg 资源组的所有作业：

   ```azurepowershell-interactive
   Get-AzImportExport -ResourceGroupName myierg
   ```

1. 若要更新作业或取消作业，请运行 [AzImportExport](/powershell/module/az.importexport/update-azimportexport) cmdlet：

   ```azurepowershell-interactive
   Update-AzImportExport -Name MyIEjob1 -ResourceGroupName myierg -CancelRequested
   ```

---

## <a name="step-3-optional-configure-customer-managed-key"></a>步骤 3（可选）：配置客户管理的密钥

如果要使用 Microsoft 托管密钥保护驱动器的 BitLocker 密钥，请跳过此步骤并转到下一步。 要配置自己的密钥以保护 BitLocker 密钥，请按照[在 Azure 门户中使用用于 Azure 导入/导出的 Azure 密钥保管库配置客户管理的密钥](storage-import-export-encryption-key-portal.md)中的说明进行操作

## <a name="step-4-ship-the-drives"></a>步骤 4：寄送驱动器

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-5-update-the-job-with-tracking-information"></a>步骤 5：使用跟踪信息更新作业

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-6-verify-data-upload-to-azure"></a>步骤 6：验证 Azure 中的数据上传

跟踪作业直至完成。 作业完成后，验证数据已上传到 Azure。 仅在已确认上传成功后才删除本地数据。

## <a name="next-steps"></a>后续步骤

* [查看作业和驱动器状态](storage-import-export-view-drive-status.md)
* [查看导入/导出要求](storage-import-export-requirements.md)
