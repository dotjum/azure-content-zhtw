<properties 
	pageTitle="使用複製活動移動資料 | Microsoft Azure" 
	description="了解 Data Factory 管線中的資料移動︰雲端存放區之間和內部部署與雲端之間的資料移轉。使用複製活動。" 
	keywords="複製資料, 資料移動, 資料移轉, 傳輸資料"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/08/2016" 
	ms.author="spelluru"/>

# 使用複製活動移動資料

## Overview
Azure Data Factory 可讓您使用複製活動，將不同圖形的資料從各種內部部署和雲端資料來源複製到 Azure，以讓資料可以進一步轉換和分析。複製活動也可用來發佈商業智慧 (BI) 和應用程式取用的轉換和分析結果。

![複製活動的角色](media/data-factory-data-movement-activities/copy-activity.png)

複製活動則由安全、可靠、可調整且[全域可用的服務](#global)來支援。本文提供關於在 Data Factory 和複製活動中移動資料的詳細資訊。首先，讓我們看看在兩個雲端資料存放區之間以及在內部部署資料存放區與雲端資料存放區之間是如何進行資料移轉的。

> [AZURE.NOTE] 若要了解活動的概觀，請參閱[了解管線和活動](data-factory-create-pipelines.md)一文。

### 在兩個雲端資料存放區之間複製資料
當來源和接收 (目的地) 資料存放區同時位於雲端時，複製活動就會經歷下列階段，將資料從來源複製/移動到接收資料存放區。支援複製活動的服務會執行下列作業：

1. 從來源資料存放區讀取資料
2. 根據輸入資料集、輸出資料集和複製活動的組態，執行序列化/還原序列化、壓縮/解壓縮、資料行對應及類型轉換
3.	將資料寫入目的地資料存放區

服務會自動選擇最佳區域來執行資料移動，這通常是最接近接收資料存放區的區域。

![從雲端複製到雲端](./media/data-factory-data-movement-activities/cloud-to-cloud.png)


### 在內部部署資料存放區和雲端資料存放區之間複製資料
若要安全地在公司防火牆背後的內部部署資料存放區和雲端資料存放區之間移動資料，您必須安裝資料管理閘道，這是一個代理程式，能夠在內部部署的電腦上啟用混合式資料移動及處理。資料管理閘道可以和資料存放區本身安裝於同一部電腦上，或者安裝於具備可觸達該資料存放區之存取權的個別電腦上。在此案例中，序列化/還原序列化、壓縮/解壓縮、資料行對應及類型轉換都是透過資料管理閘道來執行。資料不流經 Azure Data Factory 服務就是這種情況。資料管理閘道會直接將資料寫入到目的地存放區。

![從內部部署複製到雲端](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

如需指示和逐步解說，請參閱[在內部部署與雲端資料存放區之間移動資料](data-factory-move-data-between-onprem-and-cloud.md)一文；如需資料管理閘道的詳細資訊，請參閱[資料管理閘道](data-factory-data-management-gateway.md)一文。

您也可以使用資料管理閘道，將資料移出/移入裝載於 Azure IaaS VM (基礎結構即為服務的虛擬機器) 上支援的資料存放區。在此情況下，資料管理閘道可以和資料存放區本身安裝於同一部 Azure VM 上，或者安裝於具備可觸達該資料存放區之存取權的個別 VM 上。

## 支援的資料存放區和格式
複製活動會將資料從**來源**資料存放區複製到**接收**資料存放區。Data Factory 支援下列資料存放區，且**來自任何來源的資料都可以寫入任何接收**。按一下資料存放區以了解如何從該存放區複製資料以及將資料複製到該存放區。

類別 | 資料存放區 | 支援做為來源 | 支援做為接收
:------- | :--------- | :------------------ | :-----------------
Azure | [Azure Blob](data-factory-azure-blob-connector.md) <br/> [Azure Data Lake Store](data-factory-azure-datalake-connector.md) <br/> [Azure SQL Database](data-factory-azure-sql-connector.md) <br/> [Azure SQL 資料倉儲](data-factory-azure-sql-data-warehouse-connector.md) <br/> [Azure 資料表](data-factory-azure-table-connector.md) <br/> [Azure DocumentDB](data-factory-azure-documentdb-connector.md) <br/> | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ 
資料庫 | [SQL Server](data-factory-sqlserver-connector.md)* <br/> [Oracle](data-factory-onprem-oracle-connector.md)* <br/> [MySQL](data-factory-onprem-mysql-connector.md)* <br/> [DB2](data-factory-onprem-db2-connector.md)* <br/> [Teradata](data-factory-onprem-teradata-connector.md)* <br/> [PostgreSQL](data-factory-onprem-postgresql-connector.md)* <br/> [Sybase](data-factory-onprem-sybase-connector.md)* <br/>[Cassandra](data-factory-onprem-cassandra-connector.md)* <br/>[MongoDB](data-factory-on-premises-mongodb-connector.md)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓<br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;<br/> &nbsp; 
檔案 | [檔案系統](data-factory-onprem-file-system-connector.md)* <br/> [Hadoop 分散式檔案系統 (HDFS)](data-factory-hdfs-connector.md)* | ✓ <br/> ✓ <br/> | ✓ <br/> &nbsp; 
其他 | [Salesforce](data-factory-salesforce-connector.md)<br/> [泛型 ODBC](data-factory-odbc-connector.md)* <br/> [泛型 OData](data-factory-odata-connector.md) <br/> [Web 資料表 (來自 HTML 的資料表)](data-factory-web-table-connector.md) <br/> [GE Historian](data-factory-odbc-connector.md#ge-historian-store)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;

> [AZURE.NOTE] 具有 * 的資料存放區可以在內部部署環境或 Azure IaaS，並且需要您在內部部署/Azure IaaS 電腦上安裝[資料管理閘道](data-factory-data-management-gateway.md)。

如果您需要將資料移至**複製活動**不支援的資料存放區，或從該資料存放區移動資料，則您可以在 Data Factory 中使用**自訂活動**搭配自己的邏輯來複製/移動資料。如需建立及使用自訂活動的詳細資料，請參閱[在 Azure Data Factory 管線中使用自訂活動](data-factory-use-custom-activities.md)文章。

### 支援的檔案格式
複製活動可以在兩個檔案型資料存放區 (例如 Azure Blob、檔案系統和 Hadoop 分散式檔案系統 (HDFS)) 之間原封不動地複製檔案。若要這樣做，您可以在輸入和輸出資料集定義中同時略過[格式化區段](data-factory-create-datasets.md)，如此一來，就不必進行任何序列化/還原序列化作業，而可以有效率地複製資料。

複製活動也會以指定格式讀取和寫入檔案︰文字、Avro、ORC 和 JSON。以下是您可以實現的一些複製活動範例︰

-	從 Azure Blob 複製文字 (CSV) 格式的資料並寫入至 Azure SQL
-	從檔案系統內部部署環境複製文字 (CSV) 格式的檔案並以 Avro 格式寫入至 Azure Blob
-	複製 Azure SQL Database 中的資料並以 ORC 格式寫入至 HDFS 內部部署環境



## <a name="global"></a>全域可用的資料移動
即使 Azure Data Factory 本身只能在美國西部、美國東部和北歐地區使用，但是服務支援的複製活動可供下列區域和地理位置全域使用。全域可用性的拓撲可確保有效資料移動，避免通常會發生的跨區域躍點。如需某區域中是否可使用 Data Factory 服務和資料移動的資訊，請參閱[依區域提供的服務](https://azure.microsoft.com/regions/#services)。

### 在雲端資料存放區之間複製資料
當來源和接收資料存放區都位於雲端時，Azure Data Factory 會使用區域中最接近相同地理位置之接收位置的服務部署，來執行資料移動。如需對應資訊，請參閱下表︰

目的地資料存放區的區域 | 用於資料移動的區域
:----------------------------------- | :----------------------------
美國東部 | 美國東部
美國東部 2 | 美國東部 2
美國中部 | 美國中部
美國西部 | 美國西部
美國中北部 | 美國中北部
美國中南部 | 美國中南部
北歐 | 北歐
西歐 | 西歐
東南亞 | 東南亞
東亞 | 東南亞
日本東部 | 日本東部
日本西部 | 日本東部
巴西南部 | 巴西南部
澳洲東部 | 澳洲東部
澳大利亞東南部 | 澳大利亞東南部
印度中部 | 印度中部
印度南部 | 印度中部
印度西部 | 印度中部


> [AZURE.NOTE] 如果目的地資料存放區的區域不在上述清單中，複製活動將會失敗而不是前往替代區域。

### 在內部部署資料存放區和雲端資料存放區之間複製資料
在內部部署環境 (或 Azure IaaS VM) 和雲端之間複製資料時，會由內部部署電腦或 Azure IaaS VM 上的[資料管理閘道](data-factory-data-management-gateway.md)執行資料移動。資料不會流經雲端服務，除非您使用[分段複製](data-factory-copy-activity-performance.md#staged-copy)功能，在此情況下，資料會先流經暫存 Azure Blob 儲存體，再寫入至接收資料存放區。


## 建立具有複製活動的管線 
您可以使用兩種方式來建立具有複製活動的管線︰

### 使用複製精靈
**Data Factory 複製精靈**可讓您建立具有複製活動的管線，以在**不需要撰寫連結服務、資料集和管線之 JSON** 定義的情況下，將資料從支援的來源複製到目的地。如需精靈的詳細資訊，請參閱 [Data Factory 複製精靈](data-factory-copy-wizard.md)教學課程。

### 使用 JSON 指令碼
您可以使用 Azure 入口網站中的 Data Factory 編輯器 (或) Visual Studio (或) Azure PowerShell，為具有複製活動的管線建立 JSON 定義，並加以部署以在 Data Factory 中建立管線。如需含有逐步指示的教學課程，請參閱[教學課程：在 Azure Data Factory 管線中使用複製活動](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。

名稱、描述、輸入和輸出資料表、原則等 JSON 屬性都適用於所有活動類型。另一方面，活動的 **typeProperties** 區段中可用的屬性會隨著每個活動類型而有所不同。

以複製活動而言，**typeProperties** 區段會根據來源和接收的類型而有所不同。按一下[支援來源/接收](#supported-data-stores)一節的來源/接收，了解該資料存放區複製活動所支援的類型屬性。

以下是範例 JSON 定義︰

	{
	  "name": "ADFTutorialPipeline",
	  "properties": {
	    "description": "Copy data from Azure blob to Azure SQL table",
	    "activities": [
	      {
	        "name": "CopyFromBlobToSQL",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "InputBlobTable"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "OutputSQLTable"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource"
	          },
	          "sink": {
	            "type": "SqlSink",
	            "writeBatchSize": 10000,
	            "writeBatchTimeout": "60:00:00"
	          }
	        },
	        "Policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "NewestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	    ],
	    "start": "2016-07-12T00:00:00Z",
	    "end": "2016-07-13T00:00:00Z"
	  }
	} 

輸出資料集中定義的排程會決定活動執行時間 (例如**每日**︰頻率︰日、間隔︰1)。活動會將輸入資料集 (**來源**) 的資料複製到輸出資料集 (**接收**)。您可以指定多個輸入資料集給複製活動，以便用來先確認相依性，再執行活動，但只有第一個資料集的資料會複製到目的地資料集。如需詳細資訊，請參閱[排程和執行](data-factory-scheduling-and-execution.md)。

## 效能和微調 
請參閱「[複製活動的效能及微調指南](data-factory-copy-activity-performance.md)」一文，其中說明在 Azure Data Factory 中會影響資料移動 (複製活動) 效能的重要因素。它也列出在內部測試期間所觀察到的效能，並討論要讓複製活動效能最佳化的各種方式。

## 排程和循序複製
請參閱[排程和執行](data-factory-scheduling-and-execution.md)一文，以取得排程和執行在 Data Factory 中之運作方式的詳細資訊。

您可以利用循序/排序的方式，逐一執行多個複製作業。請參閱文章中的[已排序的複製](data-factory-scheduling-and-execution.md#ordered-copy)一節。

## 類型轉換 
不同的資料存放區有不同的原生類型系統。複製活動會利用下列含有兩個步驟的方法執行自動類型轉換，從來源類型到接收類型：

1. 從原生來源類型轉換成 .NET 類型
2. 從 .NET 類型轉換成原生接收類型

您可以在各自的資料存放區特定文章中找到資料存放區的指定原生類型系統對 .NET 的對應 (按一下[支援的資料存放區](#supported-data-stores)資料表中的個別連結)。您可以在建立資料表時使用這些對應來判斷適當的類型，如此就能在複製活動期間執行正確的轉換。

 
## 後續步驟
- 若要了解如何使用複製活動將資料從來源資料存放區移動到一般接收資料存放區，請參閱[從 Azure Blob 複製資料到 Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)。
- 若要了解將資料從內部部署資料存放區移動到雲端資料存放區的相關資訊，請參閱[將資料從內部部署資料存放區移動到雲端資料存放區](data-factory-move-data-between-onprem-and-cloud.md)。
 

<!---HONumber=AcomDC_0817_2016-->