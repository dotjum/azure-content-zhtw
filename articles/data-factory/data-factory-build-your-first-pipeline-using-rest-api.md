<properties
	pageTitle="建置您的第一個 Data Factory (REST) |Microsoft Azure"
	description="在本教學課程中，您會使用 Data Factory REST API 建立範例 Azure Data Factory 管線。"
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="jhubbard"
	editor="monicar"
/>

<tags
	ms.service="data-factory"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="08/16/2016"
	ms.author="spelluru"/>

# 使用 Data Factory REST API 建置您的第一個 Azure Data Factory
> [AZURE.SELECTOR]
- [教學課程概觀](data-factory-build-your-first-pipeline.md)
- [使用 Data Factory 編輯器](data-factory-build-your-first-pipeline-using-editor.md)
- [使用 PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [使用 Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [使用 Resource Manager 範本](data-factory-build-your-first-pipeline-using-arm.md)
- [使用 REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

在本文中，您會了解如何使用 Data Factory REST API 來建立您的第一個 Azure Data Factory。

## 必要條件

- 詳閱[教學課程概觀](data-factory-build-your-first-pipeline.md)一文。本文協助您了解 Azure Data Factory 的基本概念。
- 在您的電腦上安裝 [Curl](https://curl.haxx.se/dlwiz/)。您可搭配使用 CURL 工具與 REST 命令來建立 Data Factory。
- 請依照[本文](../resource-group-create-service-principal-portal.md)的指示來執行下列作業：
	1. 在 Azure Active Directory 中建立名為 **ADFGetStartedApp** 的 Web 應用程式。
	2. 取得**用戶端識別碼**和**秘密金鑰**。
	3. 取得**租用戶識別碼**。
	4. 將 **ADFGetStartedApp** 應用程式指派給 **Data Factory 參與者**角色。
- 安裝 [Azure PowerShell](../powershell-install-configure.md)。
- 啟動 **PowerShell** 並執行下列命令。將 Azure PowerShell 維持在開啟狀態，直到本教學課程結束為止。如果您關閉並重新開啟，則需要再次執行這些命令。
	1. 執行 **Login-AzureRmAccount**，並輸入您用來登入 Azure 入口網站的使用者名稱和密碼。
	2. 執行 **Get-AzureRmSubscription** 以檢視此帳戶的所有訂用帳戶。
	3. 執行 **Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription| Set-AzureRmContext** to select the subscription that you want to work with. Replace **NameOfAzureSubscription** with the name of your Azure subscription. 
3. 在 PowerShell 中執行以下命令，建立名為 **ADFTutorialResourceGroup** 的 Azure 資源群組。

		New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

	本教學課程的某些步驟假設您使用名為 ADFTutorialResourceGroup 的資源群組。如果使用不同的資源群組，您必須以資源群組的名稱取代本教學課程中的 ADFTutorialResourceGroup。

## 建立 JSON 定義
在 curl.exe 所在的資料夾中建立下列 JSON 檔案。

### datafactory.json 
> [AZURE.IMPORTANT] 名稱必須是全域唯一的，所以建議您使用 ADFCopyTutorialDF 做為前置詞/後置詞，使其成為唯一的名稱。

	{  
	    "name": "FirstDataFactoryREST",  
	    "location": "WestUS"
	}  

### azurestoragelinkedservice.json
> [AZURE.IMPORTANT] 以 Azure 儲存體帳戶的名稱和金鑰取代 **accountname** 和 **accountkey**。若要了解如何取得儲存體存取金鑰，請參閱[檢視、複製和重新產生儲存體存取金鑰](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

	{
	    "name": "AzureStorageLinkedService",
	    "properties": {
	        "type": "AzureStorage",
	        "typeProperties": {
	            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	        }
	    }
	}


### hdinsightondemandlinkedservice.json

	{
		"name": "HDInsightOnDemandLinkedService",
		"properties": {
			"type": "HDInsightOnDemand",
			"typeProperties": {
				"version": "3.2",
				"clusterSize": 1,
				"timeToLive": "00:30:00",
				"linkedServiceName": "AzureStorageLinkedService"
			}
		}
	}

下表提供程式碼片段中所使用之 JSON 屬性的描述：

| 屬性 | 說明 |
| :------- | :---------- |
| 版本 | 指定所建立的 HDInsight 版本為 3.2。 | 
| ClusterSize | HDInsight 叢集的大小。 | 
| TimeToLive | 指定 HDInsight 叢集在被刪除之前的閒置時間。 |
| linkedServiceName | 指定用來儲存 HDInsight 產生之記錄檔的儲存體帳戶 |

請注意：

- Data Factory 會以上述 JSON 為您建立「以 Windows 為基礎的」HDInsight 叢集。您也可以讓它建立**以 Linux 為基礎的** HDInsight 叢集。如需詳細資訊，請參閱 [HDInsight 隨選連結服務](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。
- 您可以使用**自己的 HDInsight 叢集**，不必使用隨選的 HDInsight 叢集。如需詳細資訊，請參閱 [HDInsight 連結服務](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)。
- HDInsight 叢集會在您於 JSON 中指定的 Blob 儲存體 (**linkedServiceName**) 建立**預設容器**。HDInsight 不會在刪除叢集時刪除此容器。這是設計的行為。在使用 HDInsight 隨選連結服務時，除非有現有的即時叢集 (**timeToLive**)，否則每當需要處理配量時，就會建立 HDInsight 叢集，並在處理完成時予以刪除。

	隨著處理的配量越來越多，您會在 Azure Blob 儲存體中看到許多容器。如果在疑難排解作業時不需要這些容器，建議您加以刪除以降低儲存成本。這些容器的名稱遵循下列模式："adf**yourdatafactoryname**-**linkedservicename**-datetimestamp"。請使用 [Microsoft 儲存體總管](http://storageexplorer.com/)之類的工具，來刪除 Azure Blob 儲存體中的容器。

如需詳細資訊，請參閱 [HDInsight 隨選連結服務](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。

### inputdataset.json

	{
		"name": "AzureBlobInput",
		"properties": {
			"type": "AzureBlob",
			"linkedServiceName": "AzureStorageLinkedService",
			"typeProperties": {
				"fileName": "input.log",
				"folderPath": "adfgetstarted/inputdata",
				"format": {
					"type": "TextFormat",
					"columnDelimiter": ","
				}
			},
			"availability": {
				"frequency": "Month",
				"interval": 1
			},
			"external": true,
			"policy": {}
		}
	}


JSON 會定義名為 **AzureBlobInput** 的資料集，以表示管線中活動的輸入資料。此外，它也會指定將輸入資料放在名為 **adfgetstarted** 的 Blob 容器及名為 **inputdata** 的資料夾中。

下表提供程式碼片段中所使用之 JSON 屬性的描述：

| 屬性 | 說明 |
| :------- | :---------- |
| 類型 | 類型屬性設為 AzureBlob，因為資料位於 Azure Blob 儲存體。 |  
| linkedServiceName | 表示您稍早建立的 StorageLinkedService。 |
| fileName | 這是選用屬性。如果您省略此屬性，會挑選位於 folderPath 的所有檔案。在這種情況下，只會處理 input.log。 |
| 類型 | 記錄檔為文字格式，因此我們會使用 TextFormat。 | 
| columnDelimiter | 記錄檔案中的資料行會以逗號字元 (,) 分隔 |
| frequency/interval | 頻率設為「每月」且間隔為 1，表示每個月都會可取得輸入配量。 | 
| external | 如果輸入資料不是由 Data Factory 服務產生，此屬性會設為 true。 | 

### outputdataset.json

	{
		"name": "AzureBlobOutput",
		"properties": {
			"type": "AzureBlob",
			"linkedServiceName": "AzureStorageLinkedService",
			"typeProperties": {
				"folderPath": "adfgetstarted/partitioneddata",
				"format": {
					"type": "TextFormat",
					"columnDelimiter": ","
				}
			},
			"availability": {
				"frequency": "Month",
				"interval": 1
			}
		}
	}

JSON 會定義名為 **AzureBlobOutput** 的資料集，以表示管線中活動的輸出資料。此外，它也會指定將結果儲存在名為 **adfgetstarted** 的 Blob 容器及名為 **partitioneddata** 的資料夾中。**availability** 區段指定每個月產生一次輸出資料集。

### pipeline.json
> [AZURE.IMPORTANT] 以您的 Azure 儲存體帳戶名稱取代 **storageaccountname**。


	{
		"name": "MyFirstPipeline",
		"properties": {
			"description": "My first Azure Data Factory pipeline",
			"activities": [{
				"type": "HDInsightHive",
				"typeProperties": {
					"scriptPath": "adfgetstarted/script/partitionweblogs.hql",
					"scriptLinkedService": "AzureStorageLinkedService",
					"defines": {
						"inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
						"partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
					}
				},
				"inputs": [{
					"name": "AzureBlobInput"
				}],
				"outputs": [{
					"name": "AzureBlobOutput"
				}],
				"policy": {
					"concurrency": 1,
					"retry": 3
				},
				"scheduler": {
					"frequency": "Month",
					"interval": 1
				},
				"name": "RunSampleHiveActivity",
				"linkedServiceName": "HDInsightOnDemandLinkedService"
			}],
			"start": "2016-07-10T00:00:00Z",
			"end": "2016-07-11T00:00:00Z",
			"isPaused": false
		}
	}

您會在 JSON 程式碼片段中建立一個管線，其中包括在 HDInsight 叢集上使用 Hive 處理資料的單一活動。

Hive 指令碼檔案 **partitionweblogs.hql** 儲存於 Azure 儲存體帳戶 (透過 scriptLinkedService 指定，名為 **StorageLinkedService**)，且位於 **adfgetstarted**容器的 **script** 資料夾中。

**defines** 區段可用來指定執行階段設定，該設定將傳遞到 Hive 指令碼做為 Hive 設定值 (例如 ${hiveconf:inputtable}、${hiveconf:partitionedtable})。

管線的 **start** 和 **end** 屬性會指定管線的作用中期間。

在活動 JSON 中，您會指定 Hive 指令碼要在透過 **linkedServiceName** – **HDInsightOnDemandLinkedService** 指定的計算上執行。

> [AZURE.NOTE] 如需上述範例中使用的 JSON 屬性的詳細資訊，請參閱[管線的剖析](data-factory-create-pipelines.md#anatomy-of-a-pipeline)。

## 設定全域變數

在 Azure PowerShell 中，將值取代為您自己的值之後，執行下列命令：

> [AZURE.IMPORTANT] 如需取得用戶端識別碼、用戶端密碼、租用戶識別碼及訂用帳戶識別碼的指示，請參閱[必要條件](#prerequisites)一節。

	$client_id = "<client ID of application in AAD>"
	$client_secret = "<client key of application in AAD>"
	$tenant = "<Azure tenant ID>";
	$subscription_id="<Azure subscription ID>";

	$rg = "ADFTutorialResourceGroup"
	$adf = "FirstDataFactoryREST"



## 使用 AAD 驗證

	$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
	$responseToken = Invoke-Command -scriptblock $cmd;
	$accessToken = (ConvertFrom-Json $responseToken).access_token;
	
	(ConvertFrom-Json $responseToken) 



## 建立 Data Factory

在此步驟中，您會建立名為 **FirstDataFactoryREST** 的 Azure Data Factory。資料處理站可以有一或多個管線。其中的管線可以有一或多個活動。例如，「複製活動」會從來源複製資料到目的地資料存放區，HDInsight Hive 活動則是執行 Hive 指令碼轉換輸入資料以產生輸出資料。執行以下命令以建立 Data Factory：

1. 將命令指派給名為 **cmd** 的變數。

	確認您在此指定的名稱 (ADFCopyTutorialDF) 符合在 **datafactory.json** 指定的名稱。

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. 使用 **Invoke-Command** 執行命令。

		$results = Invoke-Command -scriptblock $cmd;
3. 檢視結果。如果已成功建立 Data Factory，您會在**結果**中看到 Data Factory 的 JSON；不然，您會看到一則錯誤訊息。

		$results

請注意：
 
- Azure Data Factory 的名稱在全域必須是唯一的。如果您在結果中看到錯誤︰「Data factory 名稱 “FirstDataFactoryREST” 無法使用」，請執行下列動作︰
	1. 在 **datafactory.json** 檔案中變更名稱 (例如，yournameFirstDataFactoryREST)。請參閱 [Data Factory - 命名規則](data-factory-naming-rules.md)主題，以了解 Data Factory 成品的命名規則。
	2. 在指派 **$cmd** 變數值的第一個命令中，以新的名稱取代 FirstDataFactoryREST 並執行命令。
	3. 執行下面兩個命令來叫用 REST API，以建立 Data Factory 和列印作業的結果。
- 若要建立 Data Factory 執行個體，您必須是 Azure 訂用帳戶的參與者/系統管理員
- Data Factory 的名稱未來可能會註冊為 DNS 名稱，因此會變成公開可見的名稱。
- 如果您收到錯誤：「此訂用帳戶未註冊為使用命名空間 Microsoft.DataFactory」，請執行下列其中一項，然後嘗試再次發佈︰

	- 在 Azure PowerShell 中，執行下列命令以註冊 Data Factory 提供者。
		
			Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
	
		您可以執行下列命令來確認已註冊 Data Factory 提供者。
	
			Get-AzureRmResourceProvider
	- 使用 Azure 訂用帳戶登入 [Azure 入口網站](https://portal.azure.com)並瀏覽至 [Data Factory] 刀鋒視窗 (或) 在 Azure 入口網站中建立 Data Factory。此動作會自動為您註冊提供者。

建立管線之前，您必須先建立一些 Data Factory 項目。首先，您要先建立連結的服務，以便將資料存放區/電腦連結到您的資料存放區；並定義輸入和輸出資料集，表示資料位於連結的資料存放區中，再建立具有使用這些資料集的活動之管線。

## 建立連結服務 
在此步驟中，您會將您的 Azure 儲存體帳戶和隨選 Azure HDInsight 叢集連結到您的 Data Factory。Azure 儲存體帳戶會保留此範例中管線的輸入和輸出資料。HDInsight 連結服務會用來執行此範例中管線活動指定的 Hive 指令碼。

### 建立 Azure 儲存體連結服務
在此步驟中，您會將您的 Azure 儲存體帳戶連結到您的 Data Factory。在本教學課程中，您會使用相同的 Azure 儲存體帳戶來存放輸入/輸出資料及 HQL 指令碼檔案。

1. 將命令指派給名為 **cmd** 的變數。

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. 使用 **Invoke-Command** 執行命令。
 
		$results = Invoke-Command -scriptblock $cmd;
3. 檢視結果。如果已成功建立連結服務，您會在**結果**中看到連結服務的 JSON；不然，您會看到一則錯誤訊息。
  
		$results

### 建立 Azure HDInsight 連結服務
在此步驟中，您可將隨選 HDInsight 叢集連結至 Data Factory。HDInsight 叢集會在執行階段自動建立，並在處理完成之後刪除，且會閒置一段時間。您可以使用自己的 HDInsight 叢集，不必使用隨選的 HDInsight 叢集。請參閱[計算連結服務](data-factory-compute-linked-services.md)以取得詳細資料。

1. 將命令指派給名為 **cmd** 的變數。
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. 使用 **Invoke-Command** 執行命令。

		$results = Invoke-Command -scriptblock $cmd;
3. 檢視結果。如果已成功建立連結服務，您會在**結果**中看到連結服務的 JSON；不然，您會看到一則錯誤訊息。

		$results

## 建立資料集
在此步驟中，您會建立資料集來代表 Hive 處理的輸入和輸出資料。這些資料集是您稍早在本教學課程中建立的 **StorageLinkedService**。連結的服務會指向 Azure 儲存體帳戶，而資料集則會指定保留輸入和輸出資料儲存體中的容器、資料夾和檔案名稱。

### 建立輸入資料集
在此步驟中，您會建立輸入資料集來代表 Azure Blob 儲存體中儲存的輸入資料。

1. 將命令指派給名為 **cmd** 的變數。

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. 使用 **Invoke-Command** 執行命令。

		$results = Invoke-Command -scriptblock $cmd;
3. 檢視結果。如果已成功建立資料集，您會在**結果**中看到資料集的 JSON；不然，您會看到一則錯誤訊息。
  
		$results
### 建立輸出資料集
在此步驟中，您會建立輸出資料集來代表 Azure Blob 儲存體中儲存的輸出資料。

1. 將命令指派給名為 **cmd** 的變數。
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. 使用 **Invoke-Command** 執行命令。

		$results = Invoke-Command -scriptblock $cmd;
3. 檢視結果。如果已成功建立資料集，您會在**結果**中看到資料集的 JSON；不然，您會看到一則錯誤訊息。
  
		$results 

## 建立管線
在此步驟中，您會建立第一個具有 **HDInsightHive** 活動的管線。您每個月都可取得輸入配量資訊 (頻率：每月，間隔：1)，輸出配量則是每用產生，而活動的排程器屬性也設為每用 (如下所示)。輸出資料集設定及活動排程器必須相符。目前，輸出資料集會影響排程，因此即使活動並未產生任何輸出，您都必須建立輸出資料集。如果活動沒有任何輸入，您可以略過建立輸入資料集。

確認您在 Azure Blob 儲存體的 **adfgetstarted/inputdata** 資料夾中看到了**input.log** 檔案，並執行下列命令以部署管線。由於 **start** 和 **end** 時間設定在過去，且 **isPaused** 設為 false，管線 (管線中的活動) 會在部署之後立即執行。

1. 將命令指派給名為 **cmd** 的變數。
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. 使用 **Invoke-Command** 執行命令。

		$results = Invoke-Command -scriptblock $cmd;
3. 檢視結果。如果已成功建立資料集，您會在**結果**中看到資料集的 JSON；不然，您會看到一則錯誤訊息。

		$results
5. 恭喜，您已經成功使用 Azure PowerShell 建立您的第一個管線！

## 監視管線
在此步驟中，您會使用 Data Factory REST API 來監視管線所產生的配量。

	$ds ="AzureBlobOutput"

	$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

	$results2 = Invoke-Command -scriptblock $cmd;

	IF ((ConvertFrom-Json $results2).value -ne $NULL) {
	    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
	} else {
    	    (convertFrom-Json $results2).RemoteException
	}

執行 Invoke-Command 和下一個命令，直到您看到配量處於 [就緒] 狀態或 [失敗] 狀態。當配量處於就緒狀態時，檢查您 Blob 儲存體中 **adfgetstarted** 容器內 **partitioneddata** 資料夾的輸出資料。請注意，建立隨選 HDInsight 叢集通常需要一些時間。

![輸出資料](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] 配量處理成功時就會刪除輸入檔案。因此，如果您想要重新執行配量或再次進行本教學課程，請將輸入檔案 (input.log) 上傳至 adfgetstarted 容器的 inputdata 資料夾。

您也可以使用 Azure 入口網站來監視配量及排解任何疑難問題。如需詳細，請參閱[使用 Azure 入口網站監視管線](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline)。

## Summary 
在本教學課程中，您會在 HDInsight hadoop 叢集上執行 Hive 指令碼，以建立 Azure Data Factory 來處理資料。您會在使用 Azure 入口網站中使用 Data Factory 編輯器來執行下列步驟︰

1.	建立 Azure **Data Factory**。
2.	建立兩個**連結服務**：
	1.	**Azure 儲存體**連結服務可將保留輸入/輸出檔案的 Azure Blob 儲存體連結至 Data Factory。
	2.	**Azure HDInsight** 隨選連結服務可將 HDInsight Hadoop 隨選叢集連結至 Data Factory。Azure Data Factory 會即時建立 HDInsight Hadoop 叢集以處理輸入資料及產生輸出資料。
3.	建立兩個**資料集**，以說明管線中 HDInsight Hive 活動的輸入和輸出資料。
4.	建立具有 **HDInsight Hive** 活動的**管線**。

## 後續步驟
在本文中，您已經建立可在隨選 Azure HDInsight 叢集上執行 Hive 指令碼，含有轉換活動 (HDInsight 活動) 的管線。若要了解如何使用「複製活動」從 Azure Blob 將資料複製到 Azure SQL，請參閱[教學課程：從 Azure Blob 將資料複製到 Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。

## 另請參閱
| 主題 | 說明 |
| :---- | :---- |
| [Data Factory REST API 參考](https://msdn.microsoft.com/library/azure/dn906738.aspx) | 請參閱 Data Factory Cmdlet 中的完整文件 |
| [資料轉換活動](data-factory-data-transformation-activities.md) | 本文提供 Azure Data Factory 所支援的資料轉換活動清單 (例如您在本教學課程中使用的 HDInsight Hive 轉換)。 |
| [排程和執行](data-factory-scheduling-and-execution.md) | 本文說明 Azure Data Factory 應用程式模型的排程和執行層面。 |
| [管線](data-factory-create-pipelines.md) | 本文協助您了解 Azure Data Factory 中的管線和活動，以及如何使用這些來為您的案例或業務建構端對端的資料導向工作流程。 |
| [資料集](data-factory-create-datasets.md) | 本文協助您了解 Azure Data Factory 中的資料集。
| [使用 Azure 入口網站刀鋒視窗監視及管理管線](data-factory-monitor-manage-pipelines.md) | 本文描述如何使用 Azure 入口網站刀鋒視窗來監視、管理和偵錯您的管線。 |
| [使用監視應用程式來監視和管理管線](data-factory-monitor-manage-app.md) | 本文說明如何使用監視及管理應用程式，來監視、管理管線及進行偵錯。 

<!---HONumber=AcomDC_0824_2016-->