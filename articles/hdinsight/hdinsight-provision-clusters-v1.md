<properties 
   pageTitle="在 HDInsight 中自訂佈建 Hadoop 叢集 |Microsoft Azure" 
   description="了解如何使用 Azure 傳統入口網站、Azure PowerShell、命令列或 .NET SDK 自訂佈建適用於 Azure HDInsight 的叢集。" 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

#在 HDInsight 中佈建 Hadoop 叢集 (英文)

了解如何規劃 HDInsight 叢集佈建作業。

> [AZURE.IMPORTANT] 本文件中的步驟使用 Azure 傳統入口網站。建立新的服務時，Microsoft 不建議您使用傳統入口網站。如需 Azure 入口網站的優點說明，請參閱 [Microsoft Azure 入口網站](https://azure.microsoft.com/features/azure-portal/)。
>
> 本文件也包含使用 Azure PowerShell、Azure CLI，以及適用於 HDInsight 的 .NET SDK 的相關資訊。提供的程式碼片段是以下列命令為基礎：使用 Azure 服務管理 (ASM) 來處理 HDInsight 及__已被取代__的命令。這些命令將在 2017 年 1 月 1 日之前予以移除。
>
>如需使用 Azure 入口網站的這份文件的版本，以及使用 Azure Resource Manager (ARM) 的 PowerShell、Azure CLI 及適用於 HDInsight 的 .NET SDK 程式碼片段，請參閱[在 HDInsight 中佈建 Hadoop 叢集](hdinsight-provision-clusters.md)。

**必要條件：**

開始執行本文中的指示之前，您必須擁有以下項目：

- Azure 訂用帳戶。請參閱[取得 Azure 免費試用](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。


## 基本組態選項


- **叢集名稱**

	叢集名稱用來識別叢集。叢集名稱必須遵循下列方針：
	
	- 欄位必須是包含 3 到 63 個字元的字串。
	- 欄位只能包含字母、數字和連字號。

- **訂用帳戶名稱**

	一個 HDInsight 叢集與一個 Azure 訂用帳戶綁定。
 
- **作業系統**

	您可以將 HDInsight 叢集佈建在下列兩個作業系統上：
	- **Windows 上的 HDInsight (Windows Server 2012 R2 Datacenter)**：
	- **Linux 上的 HDInsight**：HDInsight 提供在 Azure 上設定 Linux 叢集的選項。如果您熟悉 Linux 或 Unix、要從現有的 Linux Hadoop 方案進行移轉，或想輕鬆整合針對 Linux 所建置的 Hadoop 生態系統元件，請設定 Linux 叢集。如需詳細資訊，請參閱[開始在 Linux 上的 HDInsight 中使用 Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md)。


- **HDInsight 版本**

	這可用來決定要用於此叢集的 HDInsight 版本。如需詳細資訊，請參閱 [HDInsight 中的 Hadoop 叢集版本和元件](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409)。

- **叢集類型**和**叢集大小 (亦稱為資料節點)**

	HDInsight 可讓客戶部署各種不同的叢集類型，用於不同的資料分析工作負載。目前提供的叢集類型包括：
	
	- Hadoop 叢集：適用於查詢和分析工作負載
	- HBase 叢集：適用於 NoSQL 工作負載
	- Storm 叢集：適用於即時事件的處理工作負載
	- Spark 叢集：適用於記憶體內部處理、互動式查詢、串流和機器學習服務工作負載。

	![HDInsight 叢集](./media/hdinsight-provision-clusters-v1/hdinsight.clusters.png)
 
	> [AZURE.NOTE] Azure HDInsight 叢集也稱為 HDInsight 中的 Hadoop 叢集，或是 HDInsight 叢集。有時候，它可與 Hadoop 叢集互換使用。它們都代表裝載於 Microsoft Azure 環境中的 Hadoop 叢集。

	在特定叢集類型中，各節點有不同的角色，可讓客戶針對特定角色，根據適合其工作負載的詳細資料來調整節點的大小。舉例來說，若執行的分析作業類型會耗用大量記憶體，Hadoop 叢集將可使用大量記憶體來佈建背景工作節點。

	![HDInsight Hadoop 叢集角色](./media/hdinsight-provision-clusters-v1/HDInsight.Hadoop.roles.png)

	HDInsight 的 Hadoop 叢集利用兩個角色部署：

	- 前端節點 (2 個節點)
	- 資料節點 (至少 1 個節點)

	![HDInsight Hadoop 叢集角色](./media/hdinsight-provision-clusters-v1/HDInsight.HBase.roles.png)

	Hdinsight 的 HBase 叢集利用三個角色部署：
	- 前端伺服器 (2 個節點)
	- 區域伺服器 (至少 1 個節點)
	- 主要/Zookeeper 節點 (3 個節點)
	
	![HDInsight Hadoop 叢集角色](./media/hdinsight-provision-clusters-v1/HDInsight.Storm.roles.png)

	HDInsight 的 Storm 叢集利用三個角色部署：
	- Nimbus 節點 (2 個節點)
	- 監督員伺服器 (至少 1 個節點)
	- Zookeeper 節點 (3 個節點)
	

	![HDInsight Hadoop 叢集角色](./media/hdinsight-provision-clusters-v1/HDInsight.Spark.roles.png)

	適用於 HDInsight 的 Spark 叢集利用三個角色部署：
	- 前端節點 (2 個節點)
	- 背景工作角色節點 (至少 1 個節點)
	- Zookeeper 節點 (3 個節點) (對 A1 Zookeeper 而言為免費)

	客戶需根據叢集的生命期，就這些節點的使用量支付費用。一旦建立叢集之後便會開始計費，而刪除叢集時便會停止計費 (無法取消配置或保留叢集)。叢集大小會影響叢集價格。為了方便學習，建議使用 1 個資料節點。如需關於 HDInsight 定價的詳細資訊，請參閱 [HDInsight 定價](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)。


	>[AZURE.NOTE] 叢集大小限制會隨著 Azure 訂用帳戶而有所不同。若要提高限制，請與帳務支援人員連絡。
	
- **地區/虛擬網路 (即位置)**

	![Azure 區域](./media/hdinsight-provision-clusters-v1/Azure.regions.png)

	如需支援的地區清單，請按一下 [HDInsight 定價](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)中的 [地區] 下拉式清單。

- **節點大小**

	![hdinsight vm 節點大小](./media/hdinsight-provision-clusters-v1/hdinsight.node.sizes.png)

	選取 VM 的節點大小。如需詳細資訊，請參閱[雲端服務的大小](cloud-services-sizes-specs.md)

	根據選擇的 VM，您的成本可能會有所不同。HDInsight 針對叢集節點會使用所有標準層 VM。如需 VM 大小對您價格影響的相關資訊，請參閱 <a href="http://azure.microsoft.com/pricing/details/hdinsight/" target="_blank">HDInsight 定價</a>。


- **HDInsight 使用者**

	HDInsight 叢集可讓您在佈建期間設定兩個使用者帳戶：

	- HTTP 使用者。使用 Azure 傳統入口網站的基本組態時，預設的使用者名稱為 admin。
	- RDP 使用者 (Windows 叢集)：用來連線到使用 RDP 的叢集。當您建立帳戶時，必須將到期日設為從今天算起的 90 天內。
	- SSH 使用者 (Linux 叢集)：用來連線到使用 SSH 的叢集。叢集建立之後，您便可以依照[從 Linux、Unix 或 OS X 在 HDInsight 上搭配使用 SSH 與以 Linux 為基礎的 Hadoop](hdinsight-hadoop-linux-use-ssh-unix.md) 中的步驟建立其他 SSH 使用者帳戶。
  
 

- **Azure 儲存體帳戶**


	原始的 HDFS 會使用叢集上的多個本機磁碟。HDInsight 則會使用 Azure Blob 儲存體來儲存資料。Azure Blob 儲存體是強大的一般用途儲存體解決方案，其完美整合了 HDInsight。透過 Hadoop 分散式檔案系統 (HDFS) 介面，HDInsight 中的完整元件集可直接處理 Blob 儲存體中的結構化或非結構化資料。將資料儲存在 Blob 儲存體中，您便可安全地刪除用於計算的 HDInsight 叢集，而不會遺失使用者資料。
	
	在設定期間，您必須指定 Azure 儲存體帳戶，並在該 Azure 儲存體帳戶中指定 Azure Blob 儲存體容器。某些佈建程序會要求您事先建立 Azure 儲存體帳戶和 Blob 儲存體容器。叢集會以該 Blob 儲存體容器做為預設儲存位置。您也可以選擇指定叢集可存取的其他 Azure 儲存體帳戶 (連結的儲存體)。此外，叢集也可以存取任何設有完整公用讀取權限或僅限對 blob 之公用讀取權的 Blob 容器。如需限制存取的詳細資訊，請參閱[管理 Azure 儲存體資源的存取](storage-manage-access-to-resources.md)。

	![HDInsight 儲存體](./media/hdinsight-provision-clusters-v1/HDInsight.storage.png)
	
	>[AZURE.NOTE] Blob 儲存體容器提供一組 Blob 群組，如圖所示：
	
	![Azure Blob](./media/hdinsight-provision-clusters-v1/Azure.blob.storage.jpg)
	  
	
	>[AZURE.WARNING] 請不要讓多個叢集共用一個 Blob 儲存體容器。不支援此做法。
	
	如需使用次要 Blob 存放區的詳細資訊，請參閱[搭配使用 Azure Blob 儲存體與 HDInsight](hdinsight-hadoop-use-blob-storage.md)。

- **Hive/Oozie 中繼存放區**

	中繼存放區包含 Hive 和 Oozie 中繼資料，例如 Hive 資料表、資料分割、結構描述和資料行。使用中繼存放區能協助您保留 Hive 和 Oozie 中繼資料，因此在佈建新叢集時，您便無需重建 Hive 資料表或 Oozie 工作。根據預設，Hive 會使用內嵌的 Azure SQL 資料庫來儲存此資訊。刪除叢集時，嵌入的資料庫將無法保留中繼資料。舉例來說，您使用 Hive 中繼存放區來佈建叢集。您已建立一些 Hive 資料表。當您刪除叢集並透過相同的 Hive 中繼存放區重建叢集之後，便可以看到您在原始的叢集中建立的 Hive 資料表。

## 進階組態選項

>[AZURE.NOTE] 本區段目前僅適用於以 Windows 為基礎的 HDInsight 叢集。

### 使用 HDInsight 叢集自訂功能來自訂叢集

有時候，您可能需要設定組態檔。以下是一些需要設定的組態檔。

- core-site.xml
- hdfs-site.xml
- mapred-site.xml
- yarn-site.xml
- hive-site.xml
- oozie-site.xml

叢集無法保留重新安裝映像所造成的變更。如需詳細資訊，請參閱[角色執行個體由於作業系統升級而重新啟動](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) (英文)。若要在叢集存留期間保留變更，您可以在佈建程序期間使用 HDInsight 叢集自訂。

以下是自訂 Hive 組態的 Azure PowerShell 指令碼範例：

	# hive-site.xml configuration 
	$hiveConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
	$hiveConfigValues.Configuration = @{ "hive.metastore.client.socket.timeout"="90" } #default 60
	
	$config = New-AzureHDInsightClusterConfig `
	            -ClusterSizeInNodes $clusterSizeInNodes `
	            -ClusterType $clusterType `
	          | Set-AzureHDInsightDefaultStorage `
	            -StorageAccountName $defaultStorageAccount `
	            -StorageAccountKey $defaultStorageAccountKey `
	            -StorageContainerName $defaultBlobContainer `
	          | Add-AzureHDInsightConfigValues `
	            -Hive $hiveConfigValues 
	
	New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $credential -OSType Windows -Config $config

以下是更多自訂其他組態檔的範例：

	# hdfs-site.xml configuration
	$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1
	
	# core-site.xml configuration
	$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50
	
	# mapred-site.xml configuration
	$MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'
	$MapRedConfigValues.Configuration = @{ "mapreduce.task.timeout"="1200000" } #default 600000
	
	# oozie-site.xml configuration
	$OozieConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightOozieConfiguration'
	$OozieConfigValues.Configuration = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
	
如需詳細資訊，請參閱 Azim Uddin 標題為[自訂 HDInsight 叢集佈建](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx)的部落格。




### 使用指令碼動作來自訂叢集

您可以在佈建期間使用指令碼來安裝其他元件或自訂組態。這類指令碼可透過 [**指令碼動作**] 叫用。指令碼動作為組態選項，於 Azure 傳統入口網站、HDInsight Windows PowerShell Cmdlet 或 HDInsight .NET SDK 提供。如需詳細資訊，請參閱[使用指令碼動作自訂 HDInsight 叢集](hdinsight-hadoop-customize-cluster.md)。


### 使用 Azure 虛擬網路

[Azure 虛擬網路](https://azure.microsoft.com/documentation/services/virtual-network/)可讓您建立安全、持續的網路，上面有您的解決方案所需的資源。虛擬網路可讓您：

* 在私人網路中將雲端資源連接在一起 (僅限雲端)。

	![diagram of cloud-only configuration](./media/hdinsight-provision-clusters-v1/hdinsight-vnet-cloud-only.png)

* 使用虛擬私人網路 (VPN) 將雲端資源連接到本機資料中心網路 (站對站，或點對站)。

	站對站組態可讓您使用硬體 VPN 或路由及遠端存取服務，從資料中心將多項資源連接至 Azure 虛擬網路。

	![diagram of site-to-site configuration](./media/hdinsight-provision-clusters-v1/hdinsight-vnet-site-to-site.png)

	點對站組態可讓您使用軟體 VPN，將特定資源連接到 Azure 虛擬網路。

	![diagram of point-to-site configuration](./media/hdinsight-provision-clusters-v1/hdinsight-vnet-point-to-site.png)

如需搭配虛擬網路使用 HDInsight 的資訊 (包含虛擬網路的特定組態需求)，請參閱[使用 Azure 虛擬網路延伸 HDInsight 功能](hdinsight-extend-hadoop-virtual-network.md)。

## 佈建工具

- Azure 傳統入口網站
- Azure PowerShell
- .NET SDK
- CLI

### 使用 Azure 傳統入口網站

您可以參考 [基本組態選項] 和 [進階組態選項] 以取得有關欄位的說明。

**使用自訂建立選項建立 HDInsight 叢集**

1. 登入 [Azure 傳統入口網站][azure-management-portal]。
2. 按一下頁面底部的 [+新增]，然後依序按一下 [資料服務]、[HDInsight]、[自訂建立]。
3. 在 [叢集詳細資料] 頁面上，輸入或選擇下列值：

	- 叢集名稱
	- 訂用帳戶名稱
	- 叢集類型
	- 作業系統
	- HDInsight 版本

4. 在 [設定叢集] 頁面上，輸入或選取下列值：

	- 資料節點
	- 區域/虛擬網路
	- 前端節點大小
	- 資料節點大小

5. 在 [Configure Cluster User] 頁面上，提供下列值：

	- HTTP 使用者名稱
	- HTTP 密碼/確認密碼
	- 為叢集啟用遠端桌面
	- 輸入 Hive/Oozie 中繼存放區

6. 如果您選擇在上一個畫面中輸入 Hive/Oozie 中繼存放區，請在 [設定 Hive/Oozie 中繼存放區] 頁面上提供下列值：

	- Hive 中繼存放區資料庫
	- 資料庫使用者
	- 資料庫使用者密碼
	- Oozie 中繼存放區資料庫
	- 資料庫使用者
	- 資料庫使用者密碼

7. 在 [儲存體帳戶] 頁面上，提供下列值：

	- 儲存體帳戶
	- 帳戶名稱
	- 預設容器
	- 其他儲存體帳戶

8. 若要在建立叢集時自訂叢集，請在 [指令碼動作] 頁面上，按一下 [加入指令碼動作] 以提供有關您將執行的自訂指令碼詳細資料。如需詳細資訊，請參閱[使用指令碼動作自訂 HDInsight 叢集](hdinsight-hadoop-customize-cluster.md)。

	- 名稱：指定指令碼動作的名稱。
	- 指令碼 URI：針對自訂叢集時所叫用的指令碼，指定統一資源識別項 (URI)。
	- 節點類型：指定執行自訂指令碼的節點。您可以選擇 [所有節點]<b></b>、[僅限前端節點]<b></b> 或 [僅限資料節點]<b></b>。
	- 參數：若指令碼要求的話，請指定參數。</td></tr>

	您可以加入一個以上的指令碼動作，以在叢集上安裝多個元件。加入指令碼之後，請按一下核取記號以開始佈建叢集。

### 使用 Azure PowerShell
Azure PowerShell 是功能強大的指令碼環境，可讓您在 Azure 中控制和自動化工作量的部署與管理。本節提供如何使用 Azure PowerShell 佈建 HDInsight 叢集的指示。如需設定工作站以執行 HDInsight Windows Powershell Cmdlet 的相關資訊，請參閱[安裝並設定 Azure PowerShell](../powershell-install-configure.md)。如需搭配使用 Azure PowerShell 與 HDInsight 的詳細資訊，請參閱[使用 PowerShell 管理 HDInsight](hdinsight-administer-use-powershell.md)。如需 HDInsight Windows PowerShell Cmdlet 的清單，請參閱 [HDInsight Cmdlet 參考資料](https://msdn.microsoft.com/library/azure/dn858087.aspx)。

> [AZURE.NOTE] 本節的指令碼可用來設定 Azure 虛擬網路上的 HDInsight 叢集，但不會建立 Azure 虛擬網路。如需建立 Azure 虛擬網路的相關資訊，請參閱[虛擬網路組態工作](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)。

以下是使用 Azure PowerShell 佈建 HDInsight 叢集時所需執行的程序：

- 建立 Azure 儲存體帳戶
- 建立 Azure Blob 容器
- 建立 HDInsight 叢集

您可以使用 Windows PowerShell 主控台或 Windows PowerShell 整合式指令碼環境 (ISE) 來執行指令碼。

HDInsight 會使用 Azure Blob 儲存容器作為預設檔案系統。必須要有 Azure 儲存體帳戶和儲存體容器，才能夠建立 HDInsight 叢集。儲存體帳戶必須與 HDInsight 叢集位於相同的資料中心。

**連接到您的 Azure 帳戶**

	Add-AzureAccount

系統會提示您輸入 Azure 帳號認證。

**建立 Azure 儲存體帳戶**

	$storageAccountName = "<StorageAcccountName>"	# Provide a Storage account name
	$location = "<MicrosoftDataCenter>"				# For example, "West US"

	# Create an Azure Storage account
	New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location

如果您已有儲存體帳戶，但不知道帳戶名稱和帳戶金鑰，可以使用下列 Windows PowerShell 命令來擷取資訊：

	# List Storage accounts for the current subscription
	Get-AzureStorageAccount

	# List the keys for a Storage account
	Get-AzureStorageKey "<StorageAccountName>"

**建立 Azure Blob 儲存體容器**

	$storageAccountName = "<StorageAccountName>"	# Provide the Storage account name
	$containerName="<ContainerName>"				# Provide a container name

	# Create a storage context object
	$storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
	$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName
	                                       -StorageAccountKey $storageAccountKey  

	# Create a Blob storage container
	New-AzureStorageContainer -Name $containerName -Context $destContext

在儲存體帳戶和 Blob 容器準備就緒後，您即可建立叢集。

**佈建 HDInsight 叢集**

> [AZURE.NOTE] Azure PowerShell Cmdlet 是唯一建議用來在 HDInsight 叢集中變更組態變數的方法。透過遠端桌面連接至叢集時對 Hadoop 組態檔所做的變更，可能會在叢集修補時遭到覆寫。透過 Azure PowerShell 所設定的組態值在叢集修補時會保留下來。

- 從 Azure PowerShell 主控台視窗執行下列命令：

		$subscriptionName = "<AzureSubscriptionName>"	  # The Azure subscription used for the HDInsight cluster to be created
	
		$storageAccountName = "<AzureStorageAccountName>" # HDInsight cluster requires an existing Azure Storage account to be used as the default file system
	
		$clusterName = "<HDInsightClusterName>"			  # The name for the HDInsight cluster to be created
		$clusterNodes = <ClusterSizeInNodes>              # The number of nodes in the HDInsight cluster
		$hadoopUserName = "<HadoopUserName>"              # User name for the Hadoop user. You will use this account to connect to the cluster and run jobs.
		$hadoopUserPassword = "<HadoopUserPassword>"
	
		$secPassword = ConvertTo-SecureString $hadoopUserPassword -AsPlainText -Force
		$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$secPassword)
	
		# Get the storage primary key based on the account name
		Select-AzureSubscription $subscriptionName
		$storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
		$containerName = $clusterName				# Azure Blob container that is used as the default file system for the HDInsight cluster
	
		# The location of the HDInsight cluster. It must be in the same data center as the Storage account.
		$location = Get-AzureStorageAccount -StorageAccountName $storageAccountName | %{$_.Location}
	
		# Create a new HDInsight cluster
		New-AzureHDInsightCluster -Name $clusterName -Credential $credential -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainerName $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop

	>[AZURE.NOTE] $hadoopUserName 和 $hadoopUserPassword 命令用來建立叢集的 Hadoop 使用者帳戶。您將使用此帳戶來連接到叢集並執行工作。如果您從 Azure 傳統入口網站使用 [快速建立] 選項佈建叢集，預設的 Hadoop 使用者名稱將為「admin」。請不要將此帳戶與遠端桌面通訊協定 (RDP) 使用者帳戶相混淆。RDP 使用者帳戶必須與 Hadoop 使用者帳戶不同。如需詳細資訊，請參閱 [使用 Azure 傳統入口網站管理 HDInsight 中的 Hadoop 叢集][hdinsight-admin-portal]。

	叢集佈建可能需要幾分鐘的時間才會完成。

	![HDI.CLI.Provision][image-hdi-ps-provision]

**使用自訂組態選項佈建 HDInsight 叢集**

佈建叢集時，您可以使用其他組態選項，例如連接至多個 Azure Blob 儲存體容器、使用虛擬網路，或使用 Azure SQL Database 來處理 Hive 和 Oozie 中繼存放區。這可讓您區隔資料和中繼資料的存留期與叢集的存留期。

> [AZURE.NOTE] Windows PowerShell Cmdlet 是唯一建議用來在 HDInsight 叢集中變更組態變數的方法。透過遠端桌面連接至叢集時對 Hadoop 組態檔所做的變更，可能會在叢集修補時遭到覆寫。透過 Azure PowerShell 所設定的組態值在叢集修補時會保留下來。

- 從 Windows PowerShell 視窗執行下列命令：

		$subscriptionName = "<SubscriptionName>"
		$clusterName = "<ClusterName>"
		$location = "<MicrosoftDataCenter>"
		$clusterNodes = <ClusterSizeInNodes>

		$storageAccountName_Default = "<DefaultFileSystemStorageAccountName>"
		$containerName_Default = "<DefaultFileSystemContainerName>"

		$storageAccountName_Add1 = "<AdditionalStorageAccountName>"

		$hiveSQLDatabaseServerName = "<SQLDatabaseServerNameForHiveMetastore>"
		$hiveSQLDatabaseName = "<SQLDatabaseDatabaseNameForHiveMetastore>"
		$oozieSQLDatabaseServerName = "<SQLDatabaseServerNameForOozieMetastore>"
		$oozieSQLDatabaseName = "<SQLDatabaseDatabaseNameForOozieMetastore>"

		# Get the virtual network ID and subnet name
		$vnetID = "<AzureVirtualNetworkID>"
		$subNetName = "<AzureVirtualNetworkSubNetName>"

		# Get the Storage account keys
		Select-AzureSubscription $subscriptionName
		$storageAccountKey_Default = Get-AzureStorageKey $storageAccountName_Default | %{ $_.Primary }
		$storageAccountKey_Add1 = Get-AzureStorageKey $storageAccountName_Add1 | %{ $_.Primary }

		$oozieCreds = Get-Credential -Message "Oozie metastore"
		$hiveCreds = Get-Credential -Message "Hive metastore"

		# Create a Blob storage container
		$dest1Context = New-AzureStorageContext -StorageAccountName $storageAccountName_Default -StorageAccountKey $storageAccountKey_Default  
		New-AzureStorageContainer -Name $containerName_Default -Context $dest1Context

		# Create a new HDInsight cluster
		$config = New-AzureHDInsightClusterConfig -ClusterSizeInNodes $clusterNodes |
		    Set-AzureHDInsightDefaultStorage -StorageAccountName "$storageAccountName_Default.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Default -StorageContainerName $containerName_Default |
		    Add-AzureHDInsightStorage -StorageAccountName "$storageAccountName_Add1.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Add1 |
		    Add-AzureHDInsightMetastore -SqlAzureServerName "$hiveSQLDatabaseServerName.database.windows.net" -DatabaseName $hiveSQLDatabaseName -Credential $hiveCreds -MetastoreType HiveMetastore |
		    Add-AzureHDInsightMetastore -SqlAzureServerName "$oozieSQLDatabaseServerName.database.windows.net" -DatabaseName $oozieSQLDatabaseName -Credential $oozieCreds -MetastoreType OozieMetastore |
		        New-AzureHDInsightCluster -Name $clusterName -Location $location -VirtualNetworkId $vnetID -SubnetName $subNetName

	>[AZURE.NOTE] 用於 metastore 的 Azure SQL Database 必須能夠連線至其他 Azure 服務 (包括 Azure HDInsight)。在 Azure SQL Database 儀表板中，按一下右側的伺服器名稱。這是指執行 SQL Database 執行個體的伺服器。一旦進入伺服器檢視後，按一下 [設定]，然後在 [Azure 服務] 按一下 [是]，再按 [儲存]。

**列出 HDInsight 叢集**

- 從 Azure PowerShell 主控台視窗執行下列命令，以列出 HDInsight 叢集並確認是否已成功佈建叢集：

		Get-AzureHDInsightCluster -Name <ClusterName>


### 使用 Azure CLI

> [AZURE.NOTE] 從 2014/8/29 開始，無法使用 Azure CLI 建立叢集與 Azure 虛擬網路的關聯。

另一個佈建 HDInsight 叢集的選項是 Azure CLI。Azure CLI 會在 Node.js 中實作。此工具可在任何支援 Node.js 的平台上使用，包括 Windows、Mac 和 Linux。

如需如何使用 Azure CLI 的一般指南，請參閱 [Azure CLI](../xplat-cli-install.md)。

上述指示將引導您如何在 Linux 和 Windows 上安裝跨平台命令列，以及接著如何使用命令列來佈建叢集。

- [設定適用於 Linux 的 Azure CLI](#clilin)
- [設定適用於 Windows 的 Azure CLI](#cliwin)
- [使用 Azure CLI 佈建 HDInsight 叢集](#cliprovision)

#### <a id="clilin"></a>設定適用於 Linux 的 Azure CLI

執行下列程序來設定您的 Linux 電腦使用 Azure 命令列工具 (Azure CLI)：

- 使用 Node.js 套件管理員 (NPM) 安裝 Azure CLI
- 連線到 Azure 訂閱

**使用 NPM 安裝 Azure CLI**

1.	在 Linux 電腦上開啟終端機視窗，然後執行下列命令：

		sudo npm install -g https://github.com/azure/azure-xplat-cli/archive/hdinsight-February-18-2015.tar.gz

2.	執行下列命令以驗證安裝：

		azure hdinsight -h

	您可以在不同層級使用 *-h* 參數，以顯示說明資訊。例如：

		azure -h
		azure hdinsight -h
		azure hdinsight cluster -h
		azure hdinsight cluster create -h

**連線到您的 Azure 訂用帳戶**

使用 Azure CLI 之前，您必須先設定工作站與 Azure 之間的連線。Azure CLI 會使用您的 Azure 訂用帳戶資訊來連線至您的帳戶。這項資訊可從 Azure 的發佈設定檔案取得。接著可以匯入發佈設定檔成為 Azure CLI 後續作業所用的永久本機組態設定。您只需匯入一次發佈設定。

> [AZURE.NOTE] 發佈設定檔案包含敏感資訊。Microsoft 建議您刪除此檔案，或另採相關步驟加密包含此檔案的使用者資料夾。在 Windows 中，修改資料夾屬性或使用 BitLocker Drive Encryption。


1.	開啟終端機視窗。
2.	執行下列命令，以登入您的 Azure 訂用帳戶：

		azure account download

	![HDI.Linux.CLIAccountDownloadImport](./media/hdinsight-provision-clusters/HDI.Linux.CLIAccountDownloadImport.png)

	此命令會開啟可下載發行設定檔案的網頁。如果網頁未開啟，請按一下終端機視窗中的連結以開啟瀏覽器頁面並登入入口網站。

3.	將發行設定檔案下載至電腦。
5.	在命令提示字元視窗中執行下列命令，以匯入發佈設定檔案：

		azure account import <path/to/the/file>


#### <a id="cliwin"></a>設定適用於 Windows 的 Azure CLI

執行下列程序來設定您的 Windows 電腦使用 Azure CLI：

- 安裝 Azure CLI (使用 NPM 或 Windows Installer)
- 下載及匯入 Azure 帳戶發佈設定


Azure CLI 可使用 NPM 或 Windows Installer 進行安裝。Microsoft 建議您僅使用下列兩個選項之一來進行安裝。

**使用 NPM 安裝 Azure CLI**

1.	瀏覽至 **www.nodejs.org**。
2.	按一下 [安裝]，並依照指示使用預設設定。
3.	從您的工作站開啟 [命令提示字元] (或是 *Azure 命令提示字元*或 *VS2012 開發人員命令提示字元*)。
4.	在命令提示字元視窗中執行下列命令：

		npm install -g https://github.com/azure/azure-xplat-cli/archive/hdinsight-February-18-2015.tar.gz

	> [AZURE.NOTE] 如果您收到錯誤，指出找不到 NPM 命令，請驗證下列路徑是否在 PATH 環境變數中：<i>C:\\Program Files (x86)\\nodejs;C:\\Users[username]\\AppData\\Roaming\\npm</i> 或 <i>C:\\Program Files\\nodejs;C:\\Users[username]\\AppData\\Roaming\\npm</i>

5.	執行下列命令以驗證安裝：

		azure hdinsight -h

	您可以在不同層級使用 *-h* 參數，以顯示說明資訊。例如：

		azure -h
		azure hdinsight -h
		azure hdinsight cluster -h
		azure hdinsight cluster create -h

**使用 Windows Installer 安裝 Azure CLI**

1.	瀏覽至 **http://azure.microsoft.com/downloads/**。
2.	向下捲動至 [**命令列工具**] 區段，然後按一下 [**Azure 命令列介面**]，並依照 Web Platform Installer 精靈操作。

**下載及匯入發佈設定**

使用 Azure CLI 之前，您必須先設定工作站與 Azure 之間的連線。Azure CLI 會使用您的 Azure 訂用帳戶資訊來連線至您的帳戶。這項資訊可從 Azure 的發佈設定檔案取得。接著可以匯入發佈設定檔成為 Azure CLI 後續作業所用的永久本機組態設定。您只需匯入一次發佈設定。

> [AZURE.NOTE] 發佈設定檔案包含敏感資訊。Microsoft 建議您刪除此檔案，或另採相關步驟加密包含此檔案的使用者資料夾。在 Windows 中，修改資料夾屬性或使用 BitLocker。


1.	開啟 [命令提示字元]。
2.	執行下列命令以下載發佈設定檔案：

		azure account download

	![HDI.CLIAccountDownloadImport][image-cli-account-download-import]

	此命令會開啟可下載發行設定檔案的網頁。

3.	出現儲存檔案的提示時，請按一下 [儲存] 並提供檔案必須儲存的位置。
5.	在命令提示字元視窗中執行下列命令，以匯入發佈設定檔案：

		azure account import <path/to/the/file>

#### <a id="cliprovision"></a>使用 Azure CLI 佈建 HDInsight 叢集

以下是使用 Azure CLI 佈建 HDInsight 叢集時所需執行的程序：

- 建立 Azure 儲存體帳戶
- 佈建叢集

**建立 Azure 儲存體帳戶**

HDInsight 會使用 Azure Blob 儲存容器作為預設檔案系統。必須要有 Azure 儲存體帳戶，才能夠建立 HDInsight 叢集。儲存體帳戶必須位於相同的資料中心內。

- 在命令提示字元視窗中執行下列命令，以建立 Azure 儲存體帳戶：

		azure storage account create [options] <StorageAccountName>

	出現位置提示時，請選取可佈建 HDInsight 叢集的位置。儲存體必須與 HDInsight 叢集位於相同的位置。目前只有「東亞」、「東南亞」、「北歐」、「西歐」、「美國東部」、「美國西部」、「美國中北部」和「美國中南部」等區域可以代管 HDInsight 叢集。

如需有關使用 Azure 傳統入口網站建立 Azure 儲存體帳戶的資訊，請參閱[建立、管理或刪除儲存體帳戶](../storage/storage-create-storage-account.md)。

如果您已經有儲存體帳戶，但不知道帳戶名稱和帳戶金鑰，則可使用下列命令來擷取資訊：

	-- Lists Storage accounts
	azure storage account list

	-- Shows information for a Storage account
	azure storage account show <StorageAccountName>

	-- Lists the keys for a Storage account
	azure storage account keys list <StorageAccountName>

使用 Azure 傳統入口網站取得資訊的詳細資訊請參閱[建立、管理或刪除儲存體帳戶](../storage/storage-create-storage-account.md)的＜*作法：檢視、複製及重新產生儲存體存取金鑰*＞一節。

HDInsight 叢集同時要求儲存體帳戶內含有容器。如果您所提供的儲存體帳戶沒有容器，則 *azure hdinsight cluster create* 命令會提示您輸入容器名稱並建立容器。不過，如果您想要預先建立容器，您可以使用下列命令：

	azure storage container create --account-name <StorageAccountName> --account-key <StorageAccountKey> [ContainerName]

在儲存體帳戶和 Blob 容器準備就緒後，您即可建立叢集。

**佈建 HDInsight 叢集**

- 在命令提示字元視窗中執行下列命令：

		azure hdinsight cluster create --clusterName <ClusterName> --storageAccountName "<StorageAccountName>.blob.core.windows.net" --storageAccountKey <storageAccountKey> --storageContainer <StorageContainerName> --dataNodeCount <NumberOfNodes> --location <DataCenterLocation> --userName <HDInsightClusterUsername> --password <HDInsightClusterPassword> --osType windows

	![HDI.CLIClusterCreation][image-cli-clustercreation]


**使用組態檔佈建 HDInsight 叢集**

一般而言，您會佈建 HDInsight 叢集、執行工作，然後將此叢集刪除，以降低成本。Azure CLI 可讓您選擇將組態儲存至檔案，以便在每次佈建叢集時重複使用。

- 在命令提示字元視窗中執行下列命令：


		#Create the config file
		azure hdinsight cluster config create <file>

		#Add commands to create a basic cluster
		azure hdinsight cluster config set <file> --clusterName <ClusterName> --dataNodeCount <NumberOfNodes> --location "<DataCenterLocation>" --storageAccountName "<StorageAccountName>.blob.core.windows.net" --storageAccountKey "<StorageAccountKey>" --storageContainer "<BlobContainerName>" --userName "<Username>" --password "<UserPassword>" --osType windows

		#If required, include commands to use additional Blob storage with the cluster
		azure hdinsight cluster config storage add <file> --storageAccountName "<StorageAccountName>.blob.core.windows.net"
		       --storageAccountKey "<StorageAccountKey>"

		#If required, include commands to use a SQL database as a Hive metastore
		azure hdinsight cluster config metastore set <file> --type "hive" --server "<SQLDatabaseName>.database.windows.net"
		       --database "<HiveDatabaseName>" --user "<Username>" --metastorePassword "<UserPassword>"

		#If required, include commands to use a SQL database as an Oozie metastore
		azure hdinsight cluster config metastore set <file> --type "oozie" --server "<SQLDatabaseName>.database.windows.net"
		       --database "<OozieDatabaseName>" --user "<SQLUsername>" --metastorePassword "<SQLPassword>"

		#Run this command to create a cluster by using the config file
		azure hdinsight cluster create --config <file>

	>[AZURE.NOTE] 用於 metastore 的 Azure SQL Database 必須能夠連線至其他 Azure 服務 (包括 Azure HDInsight)。在 Azure SQL Database 儀表板中，按一下右側的伺服器名稱。這是指執行 SQL Database 執行個體的伺服器。一旦進入伺服器檢視後，按一下 [**設定**]，然後在 [**Azure 服務**] 按一下 [**是**]，再按 [**儲存**]。


	![HDI.CLIClusterCreationConfig][image-cli-clustercreation-config]


**列出並顯示叢集詳細資料**

- 使用下列命令，以列出並顯示叢集詳細資料：

		azure hdinsight cluster list
		azure hdinsight cluster show <ClusterName>

	![HDI.CLIListCluster][image-cli-clusterlisting]


**刪除叢集**

- 使用下列命令來刪除叢集：

		azure hdinsight cluster delete <ClusterName>



### 使用 HDInsight .NET SDK
HDInsight .NET SDK 提供 .NET 用戶端程式庫，讓您能夠輕鬆地從 .NET Framework 應用程式使用 HDInsight。

以下是使用 SDK 佈建 HDInsight 叢集時必須執行的程序：

- 安裝 HDInsight .NET SDK
- 建立自我簽署憑證
- 建立主控台應用程式
- 執行應用程式


**安裝 HDInsight .NET SDK**

您可以從 [NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started) 安裝最新發佈的 SDK 組建。下一個程序會顯示相關指示。

**建立自我簽署憑證**

建立自我簽署憑證，將它安裝到工作站，然後上傳至 Azure 訂用帳戶。如需指示，請參閱[建立自我簽署憑證](http://go.microsoft.com/fwlink/?LinkId=511138)。


**建立 Visual Studio 主控台應用程式**

1. 開啟 Visual Studio 2013 或 2015。

2. 從 [檔案] 功能表中，按一下 [新增]，再按 [專案]。

3. 在 [**新增專案**] 中，輸入或選取下列值：

	|屬性|值|
	|--------|-----|
	|範本|Templates/Visual C#/Windows/Console Application|
	|名稱|CreateHDICluster|

4. 按一下 [確定] 以建立專案。

5. 在 [工具] 功能表中按一下 [Nuget 套件管理員]，然後按一下 [Package Manager Console]。

6. 在主控台中執行下列命令，以安裝套件：

		Install-Package Microsoft.Azure.Common.Authentication -Pre
		Install-Package Microsoft.Azure.Management.HDInsight -Pre
		Install-Package Microsoft.Azure.Management.Resources -Pre

	這些命令會將 .NET 程式庫及其參考新增至目前的 Visual Studio 專案。

7. 在 [方案總管] 中，按兩下 **Program.cs** 加以開啟。
8. 使用下列程式碼來取代此程式碼：

		using Microsoft.Azure;
		using Microsoft.Azure.Common.Authentication;
		using Microsoft.Azure.Common.Authentication.Factories;
		using Microsoft.Azure.Common.Authentication.Models;
		using Microsoft.Azure.Management.HDInsight;
		using Microsoft.Azure.Management.HDInsight.Models;

		namespace CreateHDICluster
		{
		    internal class Program
		    {
		        private static HDInsightManagementClient _hdiManagementClient;
		
		        private static Guid SubscriptionId = new Guid("<AZURE SUBSCRIPTION ID>");
		        private const string ResourceGroupName = "<AZURE RESOURCEGROUP NAME>";

		        private const string NewClusterName = "<HDINSIGHT CLUSTER NAME>";
		        private const int NewClusterNumNodes = <NUMBER OF NODES>;
		        private const string NewClusterLocation = "<LOCATION>";  // Must match the Azure Storage account location
                private const string NewClusterType = "Hadoop";
		        private const OSType NewClusterOSType = OSType.Windows;
		        private const string NewClusterVersion = "3.2";

		        private const string NewClusterUsername = "admin";
		        private const string NewClusterPassword = "<HTTP USER PASSWORD>";
		        private const string ExistingStorageName = "<STORAGE ACCOUNT NAME>.blob.core.windows.net";
		        private const string ExistingStorageKey = "<STORAGE ACCOUNT KEY>";
		        private const string ExistingContainer = "<DEFAULT CONTAINER NAME>"; 
		
		
		        private static void Main(string[] args)
		        {
		            var tokenCreds = GetTokenCloudCredentials();
		            var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
		            
		            var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
		            resourceManagementClient.Providers.Register("Microsoft.HDInsight");
		
		            _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
		
		            CreateCluster();
		        }
		
		        public static SubscriptionCloudCredentials GetTokenCloudCredentials(string username = null, SecureString password = null)
		        {
		            var authFactory = new AuthenticationFactory();
		
		            var account = new AzureAccount { Type = AzureAccount.AccountType.User };
		
		            if (username != null && password != null)
		                account.Id = username;
		
		            var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
		
		            var accessToken =
		                authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto)
		                    .AccessToken;
		
		            return new TokenCloudCredentials(accessToken);
		        }
		
		        public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
		        {
		            return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
		        }
		
		
		        private static void CreateCluster()
		        {
		            var parameters = new ClusterCreateParameters
		            {
		                ClusterSizeInNodes = NewClusterNumNodes,
		                Location = NewClusterLocation,
		                ClusterType = NewClusterType,
		                OSType = NewClusterOSType,
		                Version = NewClusterVersion,

		                UserName = NewClusterUsername,
		                Password = NewClusterPassword,

		                DefaultStorageAccountName = ExistingStorageName,
		                DefaultStorageAccountKey = ExistingStorageKey,
		                DefaultStorageContainer = ExistingContainer
		            };
		
		            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
		        }
			}
		}
		
10. 取代類別成員值。

**執行應用程式**

在 Visual Studio 中開啟應用程式後，按 **F5** 以執行應用程式。主控台視窗會開啟並顯示應用程式的狀態。建立 HDInsight 叢集可能需要幾分鐘的時間。


##<a id="nextsteps"></a> 後續步驟
在本文中，您學到幾種佈建 HDInsight 叢集的方法。若要深入了解，請參閱下列文章：

* [開始使用 Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - 了解如何開始使用 HDInsight 叢集
* [使用 Sqoop 搭配 HDInsight](hdinsight-use-sqoop.md) - 了解如何在 HDInsight 與 SQL Database 或 SQL Server 之間複製資料
* [使用 PowerShell 管理 HDInsight](hdinsight-administer-use-powershell.md) - 了解如何使用 PowerShell 處理 HDInsight
* [以程式設計方式提交 Hadoop 工作](hdinsight-submit-hadoop-jobs-programmatically.md) - 了解如何以程式設計方式提交工作至 HDInsight
* [Azure HDInsight SDK 文件][hdinsight-sdk-documentation] - 探索 HDInsight SDK


[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-management-portal]: https://manage.windowsazure.com

<!---HONumber=AcomDC_0817_2016-->