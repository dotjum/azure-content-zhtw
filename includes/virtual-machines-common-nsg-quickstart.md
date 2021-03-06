您在 Azure 中藉由建立網路篩選器來開啟連接埠或建立端點，讓流量流向子網路或虛擬機器 (VM) 網路介面上您選擇的連接埠。您可將控制輸入和輸出流量的這些篩選器放在可接收流量的資源所附加的網路安全性群組上。

讓我們使用連接埠 80 上的 Web 流量的常見範例。一旦您的 VM 設定為在標準 TCP 連接埠 80 上為 Web 要求提供服務 (請記得啟動適當的服務，並且在 VM 上開啟任何作業系統防火牆規則)，您將會︰

1. 建立網路安全性群組。
2. 建立輸入規則允許具有下列項目的流量︰
  - 目的地連接埠範圍為 "80"
  - 來源連接埠範圍為 "*" (允許任何來源連接埠)
  - 優先順序值小於 65,500 (比預設全面涵蓋拒絕輸入規則的優先順序高)
3. 讓網路安全性群組與 VM 網路介面或子網路產生關聯。
    
您可以建立複雜的網路組態，以使用網路安全性群組和規則保護您的環境。我們的範例僅使用一個或兩個規則，允許 HTTP 流量或遠端管理。如需詳細資訊，請參閱下一節的[「相關資訊」](#more-information-on-network-security-groups)，或[什麼是網路安全性群組？](../articles/virtual-network/virtual-networks-nsg.md)

<!---HONumber=AcomDC_0810_2016------>