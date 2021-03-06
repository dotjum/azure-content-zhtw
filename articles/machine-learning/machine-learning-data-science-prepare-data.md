<properties
	pageTitle="準備增強機器學習服務的資料的工作 | Microsoft Azure"
	description="前置處理和清除資料，為用於機器學習服務做準備。"
	services="machine-learning"
	documentationCenter=""
	authors="bradsev"
	manager="paulettm"
	editor="cgronlun" />

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016" 
	ms.author="bradsev" />


# 準備增強機器學習服務的資料的工作

前置處理和清除資料是很重要的工作，必須先執行這些工作，才能有效地將資料集用於機器學習服務。未經處理的資料通常會有雜訊且不可靠，還可能會有遺漏值。使用這類資料進行模型化可能會產生誤導的結果。這些工作屬於 Team Data Science Process (TDSP)，通常會遵循用來探索及計劃所需預先處理的資料集初始探索。如需更多關於 TDSP 程序的詳細指示，請參閱 [Team Data Science Process](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) 中概述的步驟。

前置處理和清除工作，例如資料探索工作，可以在各種不同環境中實行，例如 SQL 或 Hive 或 Azure Machine Learning Studio，並使用各種工具與語言，例如 R 或 Python，取決於您的資料的儲存位置和格式。由於 CAP 本質上是反覆的，所以這些工作可以在程序工作流程中的各個步驟進行。

本文將介紹各種不同的資料處理概念和工作，讓您可以在將資料擷取到 Azure Machine Learning 前後執行這些工作。

如需在 Azure Machine Learning Studio 內執行資料探索和前置處理的範例，請參閱[在 Azure ML Studio 中前置處理資料](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/)影片。


## 為何要前置處理和清除資料？

真實世界的資料是從各種不同的來源和程序蒐集而來，其中可能包含危及資料集品質的違規或損毀資料。一般會發生的資料品質問題如下：

* **不完整**：資料缺少屬性或包含遺漏值。
* **雜訊**：資料包含錯誤的記錄或極端值。
* **不一致**：資料包含衝突的記錄或不一致之處。

資料品質是品質預測模型的必要條件。若要避免「垃圾進、垃圾出」並改善資料品質，進而提升模型效能，請務必管理資料健康狀態畫面，以便及早發現資料問題，並決定對應資料的處理和清除步驟。

## 哪些是要採用的典型資料健康狀態畫面？

我們可以藉由檢查下列項目，來檢查一般的資料品質：

* **記錄**數目。
* **屬性** (或**特性**) 數目。
* 屬性**資料類型** (名義、序數或連續)。
* **遺漏值**的數目。
* 資料的**格式正確**。
	* 如果資料是 TSV 或 CSV 格式，檢查資料行分隔符號和行分隔符號是否一律會正確分隔資料行與行。
	* 如果資料是 HTML 或 XML 格式，請根據各自的標準來檢查資料格式是否正確。
	* 可能也需要進行剖析，以從半結構化或非結構化的資料擷取結構化資訊。
* **不一致的資料記錄**。檢查是否允許值範圍。例如，如果資料包含學生 GPA，可檢查 GPA 是否位於指定範圍內 (假設是 0~4)。

當您找到資料問題時，**處理步驟**是必需的，這通常包含清除遺漏值、資料正規化、離散化、可移除和 (或) 取代可能影響資料對齊之內嵌字元的文字處理、共通欄位的混合資料類型，以及其他項目。

**Azure Machine Learning 會取用正確格式的表格式資料**。如果資料已是表格形式，則可在 ML Studio 中使用 Azure Machine Learning 直接執行資料前置處理。如果資料的格式不是表格式，假設是 XML，就可能需要進行剖析，才能將資料的格式轉換成表格式。

## 資料前置處理中有哪些主要工作？

* **資料清除**：填入值或遺漏值，偵測並移除雜訊資料和極端值。
* **資料轉換**：將資料標準化，以減少維度和雜訊。
* **資料縮減**：取樣資料記錄或屬性，以進行較簡單的資料處理。
* **資料離散化**：將連續屬性轉換為類別屬性，以方便搭配特定的機器學習服務方法來使用。
* **文字清除**：移除可能造成資料對齊錯誤的內嵌字元，例如，定位鍵分隔的資料檔中的內嵌定位鍵、可能中斷記錄的內嵌新行等。

下列各節將詳細說明一些資料處理步驟。

## 如何處理遺漏值？

若要處理遺漏值，最好先識別遺漏值的原因，以便對問題進行更好的控制。典型的遺漏值處理方法如下：

* **刪除**：移除具有遺漏值的記錄。
* **虛擬替代**：使用虛擬值來取代遺漏值，例如，對類別值使用 _unknown_，或對數值使用 0。
* **平均值替代**：如果遺漏值是數值，可使用平均值來取代遺漏值。
* **頻率替代**：如果遺漏值是類別，可使用最常用的項目來取代遺漏值。
* **迴歸替代**：利用迴歸方法，使用迴歸值來取代遺漏值。  

## 如何將資料標準化？

資料標準化會將數值範圍重新調整為指定的範圍。常用的資料正規化方法包括：

* **最小值與最大值標準化**：以線性方式將資料轉換為範圍，例如介於 0 和 1 之間時，其中最小值會調整為 0，最大值則調整為 1。
* **Z 分數標準化**：根據平均值和標準差來調整資料範圍：將資料和平均值之間的差距除以標準差。
* **小數點調整**：藉由移動屬性值的小數點來調整資料範圍。  

## 如何將資料離散化？

您可以藉由將連續值轉換成名義屬性或間隔來將資料離散化。以下提供一些執行此動作的方法：

* **等寬分類收納**：將屬性的所有可能值範圍分成 N 個同樣大小的群組，並使用分類收納號碼來指派落於某一個分類收納中的值。
* **等高分類收納**：將屬性的所有可能值範圍分成 N 個包含相同執行個體數目的群組，然後使用分類收納號碼來指派落於某一個分類收納中的值。  

## 如何縮減資料？

有各種方法可用來縮減資料大小，以便更容易處理資料。根據資料大小和網域而定，您可以套用下列方法：

* **記錄取樣**：取樣資料記錄，並且只從資料中選擇代表性子集。
* **屬性取樣**：只從資料選取一組最重要的屬性子集。  
* **彙總**：將資料分組，並儲存每個群組的數目。例如，可以將某一家連鎖餐廳在過去 20 年的每日營收數字彙總為每月營收，以縮減資料大小。  

## 如何清除文字資料？

**表格式資料中的文字欄位**可能包含影響資料行對齊和 (或) 記錄界限的字元。例如，定位鍵分隔檔案的內嵌定位鍵可能導致資料行對齊錯誤，而內嵌的新行字元會中斷記錄行。寫入/讀取文字時，不適當的文字編碼處理會造成資訊遺失，而意外引進無法讀取的字元 (例如 null)，這可能也會影響文字剖析。您可以需要仔細地進行剖析和編輯，以清除文字欄位來取得正確的對齊方式，及 (或) 從非結構化或半結構化的文字資料中擷取結構化資料。

**資料探索**可讓您檢視早期資料。在此步驟中可以發現一些資料問題，然後套用對應的方法以解決這些問題。請務必提出問題，例如，問題的來源是什麼，以及問題可能是如何引發的。這也有助於您決定必須採取以解決這些問題的資料處理步驟。深入探討某一個想要從資料衍生的類型，也可用來排列資料處理工作的優先順序。

## 參考

>*Data Mining: Concepts and Techniques* (資料採礦：觀念與技術)，第三版，Morgan Kaufmann，2011，Jiawei Han、Micheline Kamber 及 Jian Pei

<!---HONumber=AcomDC_0622_2016-->