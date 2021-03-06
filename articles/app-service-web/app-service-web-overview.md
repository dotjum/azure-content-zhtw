<properties
	pageTitle="Web Apps 概觀 | Microsoft Azure"
	description="了解 Azure App Service 如何協助您開發和裝載 Web 應用程式"
	services="app-service\web"
	documentationCenter=""
	authors="jaime-espinosa"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/25/2016"
	ms.author="rachelap"/>

# Web 應用程式概觀

「App Service Web Apps」是受到完整管理的計算平台，非常適合用來裝載網站和 Web 應用程式。Microsoft Azure 的這個[平台即服務](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) 產品可讓您專注於商務邏輯，而讓 Azure 負責處理用來執行及調整應用程式的基礎結構。

下列的 5 分鐘影片將介紹 Azure App Service Web Apps。

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

## 什麼是 App Service 中的 Web 應用程式？

在 App Service 中，「Web 應用程式」是 Azure 提供用來裝載網站或 Web 應用程式的計算資源。

計算資源可能位於共用或專用的虛擬機器 (VM)，一切視您選擇的定價層而定。應用程式的程式碼會在與其他客戶隔離開來的受管理 VM 中執行。

程式碼可以使用 [Azure App Service](../app-service/app-service-value-prop-what-is.md) 所支援的任何語言或架構，例如 ASP.NET、Node.js、Java、PHP 或 Python。您也可以在 Web 應用程式中，執行使用 [PowerShell 和其他指令碼語言](web-sites-create-web-jobs.md#acceptablefiles)的指令碼。

如需可使用 Web Apps 的典型應用程式案例的範例，請參閱 [Web 應用程式案例](https://azure.microsoft.com/documentation/scenarios/web-app/)和 [Azure App Service、雲端服務、虛擬機器及 Service Fabric 的比較](choose-web-site-cloud-service-vm.md#scenarios)的**案例和建議**一節。

## 為何要使用 Web Apps？

以下是 App Service 適用於 Web Apps 的一些主要功能︰

- **多種語言和架構** - App Service有 ASP.NET、Node.js、Java、PHP 和 Python 的頂級支援。您也可以在 App Service VM 上執行 [PowerShell 和其他指令碼或可執行檔](../app-service-web/web-sites-create-web-jobs.md)。

- **DevOps 最佳化** - 使用 Visual Studio Team Services、GitHub 或 BitBucket 設定[持續整合和部署](../app-service-web/app-service-continuous-deployment.md)。透過[測試和預備環境](../app-service-web/web-sites-staged-publishing.md)升級更新。執行 [A/B 測試](../app-service-web/app-service-web-test-in-production-get-start.md)。使用 [Azure PowerShell](../powershell-install-configure.md) 或[跨平台命令列介面 (CLI)](../xplat-cli-install.md)，在 App Service 中管理您的應用程式。
 
- **具高可用性的全域調整** - 以手動或自動方式相應[增加](../app-service-web/web-sites-scale.md)或[放大](../azure-portal/insights-how-to-scale.md)。在 Microsoft 的通用資料中心基礎結構中隨處裝載您的應用程式，而 App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) 會承諾高可用性。

- **SaaS 平台和內部部署資料的連線** - 有超過 50 種適用於企業系統 (例如 SAP、Siebel 和 Oracle)、SaaS 服務 (例如 Salesforce 和 Office 365)，以及網際網路服務 (例如 Facebook 和 Twitter) 的[連接器](../connectors/apis-list.md)可供選擇。使用[混合式連線](../biztalk-services/integration-hybrid-connection-overview.md)和 [Azure 虛擬網路](../app-service-web/web-sites-integrate-with-vnet.md)存取內部部署資料。

- **安全性和法規遵循** - App Service 為 [ISO、SOC 和 PCI 相容](https://www.microsoft.com/TrustCenter/)。

- **應用程式範本** - 從 [Azure Marketplace](https://azure.microsoft.com/marketplace/) 中的廣泛應用程式範本清單中進行選擇，以便使用精靈來安裝受歡迎的開放原始碼軟體，例如 WordPress、Joomla 和 Drupal。

- **Visual Studio 整合** - Visual Studio 中的專用工具可簡化建立、部署和偵錯的工作。

此外，Web 應用程式可以利用 [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (例如 CORS 支援) 和 [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (例如推播通知) 所提供的功能。如需 App Service 中的應用程式類型詳細資訊，請參閱 [Azure App Service 概觀](../app-service/app-service-value-prop-what-is.md)。

除了 App Service 中的 Web Apps，Azure 還提供可用來裝載網站和 Web 應用程式的其他服務。大部分的情況下，Web Apps 是最佳選擇。若是微服務架構，請考慮使用 [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)，如果您需要能更加充分地掌控程式碼執行所在的 VM，則請考慮使用 [Azure 虛擬機器](https://azure.microsoft.com/documentation/services/virtual-machines/)。如需如何在這些 Azure 服務之間做選擇的詳細資訊，請參閱 [Azure App Service、虛擬機器、Service Fabric 及雲端服務的比較](choose-web-site-cloud-service-vm.md)。

## 開始使用

若要開始在 App Service 中的新 Web 應用程式部署範例程式碼，請遵循[在 5 分鐘內將第一個 Web 應用程式部署至 Azure 中](app-service-web-get-started.md)教學課程。您將需要免費的 Azure 帳戶。

如果您想在註冊 Azure 帳戶前開始使用 Azure App Service，請移至[試用 App Service](http://go.microsoft.com/fwlink/?LinkId=523751)，即可在 App Service 中立即建立短期入門 Web 應用程式。不需要信用卡；無需承諾。

<!---HONumber=AcomDC_0803_2016-->