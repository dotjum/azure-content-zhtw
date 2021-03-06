<properties 
   pageTitle="對 U-SQL 作業進行偵錯 |Microsoft Azure" 
   description="了解如何使用 Visual Studio 對失敗的 U-SQL Vertex 進行偵錯。" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/26/2016"
   ms.author="jgao"/>



#對 U-SQL 作業進行偵錯 

了解如何使用 Azure Data Lake Visual Studio 工具，對使用者程式碼內部錯誤所造成的失敗 U-SQL 作業進行偵錯。Data Lake Visual Studio 工具可讓使用者從叢集下載已編譯程式碼和必要的 Vertex 資料，以對失敗的作業進行追蹤和偵錯。

>[AZURE.NOTE] 如果您沒有下列兩個 Windows 升級，Visual Studio 可能會停止回應或當機：[Microsoft Visual C++ 2015 Redistributable Update 2](https://www.microsoft.com/download/details.aspx?id=51682)、[Universal C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0)。


##必要條件
-	已經閱讀[開始使用](data-lake-analytics-data-lake-tools-get-started.md)一文。

## 建立及設定偵錯專案

當您在 Data Lake Visual Studio 中開啟失敗的作業時，您會收到警示。詳細的錯誤資訊會顯示在 [錯誤] 索引標籤和視窗頂端的黃色警示列。

![Azure Data Lake Analytics U-SQL 偵錯 Visual Studio 下載 Vertex](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**下載 Vertet 並建立偵錯方案**

1.	在 Visual Studio 中開啟失敗的 U-SQL 作業。
2.	按一下 [下載] 以下載所有必要的資源和輸入資料流。如果下載失敗，請按一下 [重試]。
3.	下載完成後，按一下 [開啟] 以建立本機偵錯專案。將會建立一個名為 **VertexDebug** 的新 Visual Studio 方案，其中含有名為 **LocalVertexHost** 的空白專案。

如果在 U-SQL 程式碼後置 (Script.usql.cs) 中使用使用者定義的運算子，您必須建立一個含有使用者定義運算子程式碼的類別庫 C# 專案，並包含 VertexDebug 方案中的專案。如果您已將 .dll 組件註冊到 Data Lake Analytics 資料庫，必須將組件的原始程式碼新增到 VertexDebug 方案。
 

**設定專案**

1.	在 [方案總管] 中以滑鼠右鍵按一下 [LocalVertexHost]，然後按一下 [屬性]。
2.	將輸出路徑設定為 LocalVertexHost 專案工作目錄路徑。您可以透過 LocalVertexHost 屬性取得 LocalVertexHost 專案工作目錄路徑。
3.	建置您的 C# 專案以將 .pdb 檔案放入 LocalVertexHost 專案工作目錄，或您可以手動將 .pdb 檔案複製到這個資料夾。
4.	在 [例外狀況設定] 中，檢查 [Common Language Runtime 例外狀況]︰

![Azure Data Lake Analytics U-SQL 偵錯 Visual Studio 設定](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##對作業進行偵錯

在您下載 Vertex 來建立方案並進行偵錯，以及設定環境之後，您就可以開始對您的 U-SQL 程式碼進行偵錯。

1.	在 [方案總管] 中，以滑鼠右鍵按一下 [LocalVertexHost] 專案，指向 [偵錯]，然後按一下 [開始新執行個體]。LocalVertexHost 必須設定為啟始專案。第一次您可能會看到下列可忽略的訊息。移至 [偵錯] 畫面最多可能需要一分鐘的時間。
 
    ![Azure Data Lake Analytics U-SQL 偵錯 Visual Studio 警告](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.	使用以 Visual Studio 為基礎的偵錯體驗 (監看式、變數等) 對問題進行疑難排解。
5.	在您識別問題之後，請修正程式碼、重建 C# 專案，然後再次測試它，直到所有問題解決為止。在順利完成偵錯之後，輸出視窗會顯示下列訊息：

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##重新提交作業

完成 U-SQL 程式碼偵錯之後，您可以重新提交失敗的作業。

1. 將新的 .dll 組件註冊到您的 ADLA 資料庫。

    1.	從 Data Lake Visual Studio 工具的 [伺服器總管/Cloud Explorer]，展開 [資料庫] 節點
    2.	以滑鼠右鍵按一下 [組件] 以註冊組件。
    3.	將新的 .dll 組件註冊到 ADLA 資料庫。
 
2.	或將 C# 程式碼複製到 script.usql.cs-C# 程式碼後置檔案。
3.	重新提交您的作業。

##後續步驟

- [教學課程：開始使用 Azure 資料湖分析 U-SQL 語言](data-lake-analytics-u-sql-get-started.md)
- [教學課程：使用適用於 Visual Studio 的資料湖工具開發 U-SQL 指令碼](data-lake-analytics-data-lake-tools-get-started.md)
- [針對 Azure 資料湖分析工作開發 U-SQL 使用者定義運算子](data-lake-analytics-u-sql-develop-user-defined-operators.md)

<!---HONumber=AcomDC_0817_2016-->