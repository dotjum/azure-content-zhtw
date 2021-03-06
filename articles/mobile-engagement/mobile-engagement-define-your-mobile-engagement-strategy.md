<properties
	pageTitle="定義 Mobile Engagement 策略 | Microsoft Azure"
	description="了解如何利用分析和推播通知，開始使用及最佳化 Mobile Engagement。"
	services="mobile-engagement"
	documentationCenter="Mobile"
	authors="piyushjo"
	manager="dwrede"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/19/2016"
	ms.author="piyushjo" />

# 定義 Mobile Engagement 策略

*您可以撰寫應用程式是有原因的：讓您的使用者使用它！*

我們相信您一定將許多心力放在試著讓它成為使用者喜愛的絕佳應用程式。您可能也投資了相當大的行銷預算來取得使用者。但是在初期令人感到振奮的使用高峰之後，您可能會看到使用者慢慢地停止使用您的應用程式。*這就是 Azure Mobile Engagement 的價值所在！*：把他們留住，讓您藉由測試與了解使用者想法，逐漸改善應用程式。

我們是以非常特殊的方式，以透過推播通知和應用程式內訊息吸引使用者的方式，改善保留率和使用量；訊息以及通訊內容會針對他們量身訂做，且全都是根據他們在您應用程式中的行為。我們的目標是讓您在適當的時機和正確的位置與合適的對象進行通訊。

但要做到這點，您必須先從「了解使用者」開始，然後根據使用者的行為或特性建立群組 (我們稱為客群)，然後建立與每個客群相關的通訊內容。

## Mobile Engagement 為您的目標效勞

*我們曾討論過保留率、使用量，但目的為何？*

要建立 Mobile Engagement 策略，需要先查看您的應用程式目標和關鍵效能指標 (KPI)。

從定義目標和 KPI 著手，有助於決定導向正確目標的吸引使用案例。

使用案例是您想要發起的活動簡單清單，藉由這些活動與您的使用者進行通訊，範圍從簡單的歡迎使用，到由 IT 系統觸發的非常進階公用程式通知。建構良好的使用案例，必須包含至少「事、人、時」三元素：

1. 非常短的名稱 (例如「歡迎參加活動」)。
2. **事**：訊息範例 (例如「很高興您能加入！ 請記得登入，您的第 1 個月便可免費！」)。這絕對不是一成不變的訊息，您可以隨時變更它，但它通常有助於開始思考我們想要說的內容。
3. **人**：將接收此訊息的客群 (例如「3 天前第一次啟動應用程式，並已瀏覽過登入頁面，但未登入的所有使用者」)。
	- 沒錯，透過 Azure Mobile Engagement 您可以非常輕鬆地做到 :)
	- 同樣地，這也不是固定的訊息，您可以隨時定義您的客群，但請務必及早定義您的客群區別條件，以確保收集到正確的資料。
4. **時**：活動的時間。可能是在指定的日期，或特定的動作之後，根據觸發程序而定。Mobile Engagement 提供重要的可能性數量，以正確地訂定通訊時間。

定義使用案例和區段之後，它提供指導方針來定義必須在應用程式收集的資料。這就是「標記計劃」所扮演的角色。標記計劃可讓您確保收集的資料會指定給開發人員。因此，開發人員能夠以正確的設定內嵌 Mobile Engagement，讓您能用正確的資料運作您的活動。執行測試也非常重要，可確保正確整合，而且會收集您需要的項目。

在整合基礎上，一旦發佈應用程式，身為行銷人員的您將能夠即時看到分析、區隔對象，然後開始傳送智慧型鎖定式推播通知，以在應用程式內外和使用者交流。

### 入門使用案例
1. 入門策略：根據啟動應用程式時的使用者行為，建立數個推送通知活動，以便在第一個工作階段之後，在 D+2/5/10/15 重新交流，提高第一輪的保留率。
2. 根據使用者的行為推廣新內容 (功能、文章/影片或產品)，只將資訊傳送給較有可能受吸引的使用者。
3. 為應用程式評分：目標鎖定在最有可能在市集上將您的應用程式評為 5 顆星，且少於 1% 的使用者族群。
4. 提高訂用率：將重要內容推廣給尚未看過的使用者，以增加訂用帳戶。
5. 教學課程：再也沒有強制給每個人的教學課程。為什麼不在應用程式內建置很棒的教學課程，然後只有在使用者似乎未使用或不擅長使用某項功能時，觸發應用程式內訊息？

## 吸引使用者為何需要分析？

如同您到此時可能已經發現的，只進行廣播推送通知是不夠的。Mobile Engagement 的核心概念是幫助行銷人員和開發人員，在對的時間和地點，與合適的使用者交流。若要知道這三個主要概念，務必要從您的應用程式收集分析，然後用它來區隔對象。這在行為區段能與資料庫或 CRM 或跨通道而來的資料互補時會更強大。Mobile Engagement 允許從任何地方擷取資料，並用它來鎖定正確的對象。

為了要在吸引對象時能儘可能與內容相關，具有使用者行為的知識是很重要的，即時掌握他們的狀態。收集的資料可讓行銷人員真正專注在重要的事情，以規劃使用案例，並達成行動參與策略的目標。之前提到的達成目標同時也證明，為何最佳作法其實不是收集一切的分析資訊，而是只收集可讓您專注於想要了解的事情和使用案例。這是開始、嘗試、測試和了解如何使用解決方案，以及如何解決智慧型推播通知的好方法，還能提高應用程式保留率，讓它晉升為成功案例的一員。

>[AZURE.NOTE] 請記住：太多資料會毀了資料！

### 使用案例和最佳作法

在下列章節中，我們將簡短討論我們從客戶得到的一些重要使用案例，讓您快速入門。

#### 媒體

收集使用者取用的內容類型，然後根據這種行為區隔對象，將特定類型的內容只鎖定在比較可能取用的對象。這麼做可以避免濫發垃圾郵件給整個使用者基礎，且能確保更好的保留。

#### M 商務

收集應用程式內最常被造訪的產品類別，並鎖定對象以針對使用者比較可能購買的項目推廣折扣或該類別中的新產品。目的是為了提升營收。再次強調，目標不是廣發垃圾郵件！

#### 玩遊戲

收集使用者的遊戲等級，以及在指定的期間內所花費的時間，以鎖定可能已經卡關，且比較可能藉由紅利優惠跳到下個等級的對象。

運用獎勵，對已經有段時間不玩的使用者宣傳特定的活動，試著吸引他們回來。

#### 零售

根據喜好或行為來收集對象較可能購買哪些產品或品牌的資訊，促使對象瀏覽商店，以增加購物營收。

#### 銀行

收集使用者的資料，這些使用者均在初次啟動應用程式時建立帳戶。目標是藉由鎖定式推播通知來部署歡迎使用的策略，並增加帳戶申請數。

### 如何建立絕佳的標記計劃？

標記計畫必須像是使用者路徑或一種應用程式工作流程的描述，提供所有必要標記 (資料)，必須收集這些標記才能進行足夠的分析，了解使用者行為，並正確區隔使用者基礎。這不是技術性的程序。因此，行鎖人員能夠根據他們的 Mobile Engagement 策略指定想要收集的資料。

最低限度是至少標記應用程式的所有畫面 (在 Mobile Engagement 中稱為「活動」)。如此將有助於判斷使用者路徑。

活動可以內嵌收集動作資訊的「事件」，例如按一下按鈕。這麼做可收集應用程式內的互動資訊。於是，行銷人員便能得知使用者正在瀏覽的畫面，以及他們正在做的事。

`Jobs`是一段持續時間的動作。例如，這很適合讓行銷人員了解使用者在建立帳戶或登入時，用了多少時間。這可能也很適合讓開發人員監視呼叫 Web 服務要花多少時間。

如果使用者在應用程式中遇到問題，也可以監視`Errors`。例如經常遇到連線問題。

所有這類型的資料可以利用參數來增加 (Mobile Engagement 中的「extra-information」)，讓您可以從應用程式收集動態資料。這很重要，因為可以進行精細的客群區隔。例如，行銷人員可以根據使用者取用的內容類型來區隔使用者。內容類型會是活動或事件的動態資訊。

*App-information* 是可讓您即時確認應用程式或使用者狀態的資料。這也有助於分類對象族群，並快速鎖定目標。例如，此項目可以利用 true/false 狀態，讓您知道使用者是否登入，或其訂用帳戶是否到期。

#### 標記的範例

*使用案例：區隔對象的行為，然後以合適的推播通知內容鎖定合適的使用者*

1.	傳送推播通知以推廣某類產品：收集行為資料，以便根據對象在指定期間內造訪 x 次的產品類別，或是他們已放入購物車的特定商品來區隔對象。收集的資料可讓您區隔出客群，然後傳送推播通知給合適的對象。
2.	為應用程式評分：根據對象在社交網路上分享的內容來收集資料。目的是要藉由判斷應用程式的「大使」(ambassador) 來區隔對象。大使是透過應用程式內推播通知，要求在市集中為您的應用程式評分 5 顆星的最佳對象。

	![][1]

*使用案例：宣告性資料*
1.	區隔警示新聞：收集宣告性資料，根據對象的喜好設定來區隔對象。它允許傳送特定對象真正有興趣的特定主題推播通知。
2.	根據登入狀態區隔對象。收集資料，以了解使用者是否連線，或是否建立帳戶。協助鎖定尚未登入的使用者，並傳送推播通知以吸引使用者進行轉換。![][2]

### 後續步驟

- 請瀏覽 [Mobile Engagement 概念]，進一步了解 Mobile Engagement 基本概念。
- 請瀏覽[建立 Mobile Engagement 應用程式](mobile-engagement-create.md)以在 Azure 中建立新的 Mobile Engagement 應用程式集合，以及透過 Mobile Engagement 入口網站來開始管理您的應用程式。
- 請瀏覽[最佳作法](mobile-engagement-getting-started-best-practices.md)以取得詳細資料。
- 請瀏覽[遊戲應用程式案例](mobile-engagement-gaming-scenario.md)來了解如何使用範例遊戲應用程式實作 Mobile Engagement。
- 請瀏覽[媒體應用程式案例](mobile-engagement-media-scenario.md)來了解如何使用範例媒體應用程式實作 Mobile Engagement。
- 請瀏覽[教學課程] (英文)，進一步了解實作。

<!-- Images. -->
[1]: ./media/mobile-engagement-define-your-mobile-engagement-strategy/use-case1.png
[2]: ./media/mobile-engagement-define-your-mobile-engagement-strategy/use-case2.png

<!-- URLs. -->
[Mobile Engagement 概念]: http://azure.microsoft.com/documentation/articles/mobile-engagement-concepts/
[教學課程]: http://azure.microsoft.com/documentation/articles/mobile-engagement-ios-get-started/

<!---HONumber=AcomDC_0824_2016-->