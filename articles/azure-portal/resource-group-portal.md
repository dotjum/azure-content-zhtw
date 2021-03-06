<properties 
	pageTitle="使用 Azure 入口網站管理 Azure 資源 | Microsoft Azure" 
	description="使用 Azure 入口網站和 Azure Resource Manager 來管理資源。示範如何使用儀表板和圖格來監視資源。" 
	services="azure-resource-manager,azure-portal" 
	documentationCenter="" 
	authors="tfitzmac" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="azure-resource-manager" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/03/2016" 
	ms.author="tomfitz"/>


# 透過入口網站管理 Azure 資源

> [AZURE.SELECTOR]
- [入口網站](azure-portal/resource-group-portal.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [.NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)
- [Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group/)
- [節點](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)
- [Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)
- [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

本主題示範如何使用 [Azure 入口網站](https://portal.azure.com)搭配 [Azure Resource Manager](../resource-group-overview.md) 來管理您的 Azure 資源。目前並非所有服務都支援入口網站或資源管理員。針對這些服務，您必須使用[傳統入口網站](https://manage.windowsazure.com)。如需每個服務的狀態，請參閱 [Azure 入口網站可用性圖表](https://azure.microsoft.com/features/azure-portal/availability/)。

## 管理資源群組

1. 若要查看訂用帳戶中的所有資源群組，請選取 [資源群組]。

    ![瀏覽資源群組](./media/resource-group-portal/browse-groups.png)

1. 若要建立空的資源群組，請選取 [新增]。

    ![新增資源群組](./media/resource-group-portal/add-resource-group.png)

1. 提供新資源群組的名稱和位置。選取 [建立]。

    ![建立資源群組](./media/resource-group-portal/create-empty-group.png)

1. 您可能必須選取 [重新整理] 才能查看最近建立的資源群組。

    ![重新整理資源群組](./media/resource-group-portal/refresh-resource-groups.png)

1. 若要自訂針對資源群組顯示的資訊，請選取 [資料行]。

    ![自訂資料行](./media/resource-group-portal/select-columns.png)

1. 選取要加入的資料行，然後選取 [更新]。

    ![新增資料行](./media/resource-group-portal/add-columns.png)

1. 若要了解如何將資源部署至新的資源群組，請參閱[使用 Resource Manager 範本與 Azure 入口網站來部署資源](../resource-group-template-deploy-portal.md)。

1. 若要快速存取資源群組，您可以將此刀鋒視窗釘選到您的儀表板。

    ![釘選資源群組](./media/resource-group-portal/pin-group.png)

1. 儀表板會顯示資源群組及其資源。您可以選取資源群組或其任意資源，以瀏覽至這些項目。

    ![釘選資源群組](./media/resource-group-portal/show-resource-group-dashboard.png)

## 標記資源

您可以將標籤套用至資源群組和資源，以便以邏輯方式組織您的資產。如需使用標記的相關資訊，請參閱[使用標記來組織您的 Azure 資源](../resource-group-using-tags.md)。

[AZURE.INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## 監視資源

當您選取資源時，資源刀鋒視窗會顯示預設圖形和資料表，以便監視該資源類型。您可以變更顯示的圖形和資料表，以自訂監視資源的方式。

1. 在資源的刀鋒視窗中，選取摘要下方的 [新增區段]，以新增更多的圖形和資料表。

    ![新增區段](./media/resource-group-portal/add-section.png)

1. 從圖格資源庫，選取您要在刀鋒視窗中顯示的資訊。編輯器會依照資源類型篩選圖格。選取不同的資源會變更可用的圖格。

    ![新增區段](./media/resource-group-portal/tile-gallery.png)

1. 將您需要的圖格拖曳至可用的空間。

    ![拖曳圖格](./media/resource-group-portal/drag-tile.png)

1. 選取入口網站頂端的 [完成] 之後，新檢視就是刀鋒視窗的一部分。

    ![顯示圖格](./media/resource-group-portal/show-lens.png)

1. 您可以選取區段上方的省略符號 (...)，將此刀鋒視窗的區段釘選到您的儀表板。您也可以自訂刀鋒視窗中的區段大小或完全予以移除。下圖顯示如何釘選、自訂或移除 [CPU 和記憶體] 區段。

    ![釘選區段](./media/resource-group-portal/pin-cpu-section.png)

1. 將此區段釘選到儀表板之後，您會在儀表板上看見摘要。而且，立即加以選取，您即可看到更多有關資料的詳細資訊。

    ![檢視儀表板](./media/resource-group-portal/view-startboard.png)

1. 您也可以建立多個儀表板來監視和管理您的資源，以及與您的組織中的其他人共用這些儀表板。選取 [新增儀表板]。

    ![儀表板](./media/resource-group-portal/dashboard.png)

     若要了解如何使用儀表板，請檢視 [Build Custom Dashboards in the Microsoft Azure Portal (在 Microsoft Azure 入口網站中建置自訂儀表板)](https://channel9.msdn.com/Blogs/trevor-cloud/azure-portal-dashboards) 影片。若要深入了解如何共用已發佈儀表板的存取權，請參閱[共用 Azure 儀表板](azure-portal-dashboard-share-access.md)。

## 移動資源

如果您需要將資源移至其他資源群組或其他訂用帳戶，請參閱[將資源移到新的資源群組或訂用帳戶](../resource-group-move-resources.md)。

## 鎖定資源

您可以鎖定訂用帳戶、資源群組或資源，以防止組織中的其他使用者不小心刪除或修改重要資源。如需詳細資訊，請參閱[使用 Azure 資源管理員來鎖定資源](../resource-group-lock-resources.md)。

[AZURE.INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## 檢視訂用帳戶和成本

您可以檢視訂用帳戶的相關資訊和所有資源的彙總成本。選取 [訂用帳戶] 及您想要查看的訂用帳戶。您可能只有一個訂用帳戶可選取。

![訂用帳戶)](./media/resource-group-portal/select-subscription.png)

在訂用帳戶刀鋒視窗內，您會看到完工速率。

![完工速率](./media/resource-group-portal/burn-rate.png)

還有依資源類型的成本分析。

![資源成本](./media/resource-group-portal/cost-by-resource.png)

## 匯出範本

設定資源群組之後，您可以檢視此資源群組的資源管理員範本。匯出此範本有兩個優點︰

1. 因為此範本包含所有完整的基礎結構，所以您可以輕鬆地自動進行解決方案的未來部署。

2. 您可以查看代表您的解決方案的 JavaScript 物件標記法 (JSON)，藉此熟悉範本語法。

如需逐步指引，請參閱[從現有資源匯出 Azure Resource Manager 範本](../resource-manager-export-template.md)。

## 刪除資源群組或資源

刪除資源群組會刪除其內含的所有資源。您也可以刪除資源群組內的個別資源。在刪除資源群組時應多加留意，因為可能會有其他資源群組中的資源連結至該群組。Resource Manager 不會刪除連結的資源，但是若沒有預期的資源則可能無法正常運作。

![刪除群組](./media/resource-group-portal/delete-group.png)

## 後續步驟

- 若要檢視稽核記錄檔，請參閱[使用 Resource Manager 來稽核作業](../resource-group-audit.md)。
- 若要針對部署錯誤進行疑難排解，請參閱[使用 Azure 入口網站對資源群組部署進行疑難排解](../resource-manager-troubleshoot-deployments-portal.md)。
- 若要透過入口網站部署資源，請參閱 [使用 Resource Manager 範本與 Azure 入口網站來部署資源](../resource-group-template-deploy-portal.md)。
- 若要管理資源的存取權，請參閱[使用角色指派來管理 Azure 訂用帳戶資源的存取權](../active-directory/role-based-access-control-configure.md)。

<!---HONumber=AcomDC_0803_2016-->