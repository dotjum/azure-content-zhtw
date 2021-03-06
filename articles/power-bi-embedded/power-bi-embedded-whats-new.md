<properties
   pageTitle="Power BI Embedded 新功能"
   description="取得 Power BI Embedded 新功能的最新資訊"
   services="power-bi-embedded"
   documentationCenter=""
   authors="minewiskan"
   manager="mblythe"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="07/06/2016"
   ms.author="owend"/>

# Power BI Embedded 新功能

**Power BI Embedded** 的更新會定期發行。但是，並非所有發行版本都會包含供使用者使用的新功能，某些版本著重在後端服務功能。我們將會在這裡反白顯示對於使用者的功能。請務必經常回來查看。

## 2016 年 7 月 11 日

這個版本中包含︰

-    **好消息！** Power BI Embedded service 不再是預覽 - 現在已 GA (正式推出)。
-    所有 REST API 已從 **/beta** 移至 **/v1.0**。
-    .NET 和 JavaScript SDK 已針對 **v1.0** 更新。
-    現在可以直接使用 API 金鑰來驗證 power BI API 呼叫。只有內嵌需要應用程式權杖。在這個過程中，佈建和開發權杖已在 v1.0 API 中被取代，但是它們會繼續在 Beta 版中運作，直到 12/30/2016 為止。若要深入了解，請參閱[使用 Power BI Embedded 驗證和授權](power-bi-embedded-app-token-flow.md)。
-    應用程式權杖和內嵌報告的資料列層級安全性 (RLS) 支援。若要深入了解，請參閱[資料列層級安全性和 Power BI Embedded](power-bi-embedded-rls.md)。
-    所有 **v1.0** API 呼叫的更新範例應用程式。
-    Azure SDK、PowerShell 和 CLI 的 Power BI Embedded 支援。
-    使用者可以將視覺效果資料匯出到 **.csv**。
-    Power BI Embedded 現在受到與 Microsoft Azure 相同的所有語言/地區設定的支援。若要深入了解，請參閱 [Azure - 語言](http://social.technet.microsoft.com/wiki/contents/articles/4234.windows-azure-extent-of-localization.aspx)。

<!---HONumber=AcomDC_0713_2016-->