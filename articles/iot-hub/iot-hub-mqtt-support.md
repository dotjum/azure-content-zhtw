<properties
 pageTitle="IoT 中樞的 MQTT 支援 | Microsoft Azure"
 description="IoT 中樞層級的 MQTT 支援說明"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="07/19/2016"
 ms.author="dobett"/>

# IoT 中樞的 MQTT 支援

IoT 中樞可讓裝置在連接埠 8883 使用 [MQTT v3.1.1][lnk-mqtt-org] 通訊協定來與 IoT 中樞裝置端點進行通訊。IoT 中樞要求使用 TLS/SSL 保護所有裝置通訊的安全。

## 連接到 IoT 中樞

裝置可以使用 MQTT 通訊協定連接到 IoT 中樞，方法是使用 [Microsoft Azure IoT SDK][lnk-device-sdks] 中的程式庫或直接使用 MQTT 通訊協定。

## 使用裝置用戶端 SDK

支援 MQTT 通訊協定的[裝置用戶端 SDK][lnk-device-sdks] 有提供 Java、Node.js、C 和 C# 等版本。裝置用戶端 SDK 會使用標準的 IoT 中樞連接字串來連接到 IoT 中樞。若要使用 MQTT 通訊協定，用戶端通訊協定參數必須設定為 **MQTT**。根據預設，裝置用戶端 SDK 會連接到 **CleanSession** 旗標設為 **0** 的 IoT 中樞，並使用 **QoS 1** 來與 IoT 中樞交換訊息。

當裝置連接到 IoT 中樞時，裝置用戶端 SDK 會提供方法讓裝置在 IoT 中樞傳送和接收訊息。

下表包含每一種支援語言的程式碼範例連結，並指出要用來以 MQTT 通訊協定連接到 IoT 中樞的參數。

| 語言 | 通訊協定參數 |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | azure-iot-device-mqtt |
| [Java][lnk-sample-java] | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] | MQTT\_Protocol |
| [C#][lnk-sample-csharp] | TransportType.Mqtt |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

## 直接使用 MQTT 通訊協定

如果裝置無法使用裝置用戶端 SDK，它仍可使用 MQTT 通訊協定連接到公用裝置端點。在 **CONNECT** 封包中，裝置應使用下列值：

- 針對 [ClientId] 欄位使用 **deviceId**。
- 針對 [使用者名稱] 欄位使用 `{iothubhostname}/{device_id}`，其中 {iothubhostname} 是 IoT 中樞的完整 CName。

    例如，如果您的 IoT 中樞名稱是 **contoso.azure devices.net**，而且如果您的裝置名稱是 **MyDevice01**，則完整的 **Username** 欄位應包含 `contoso.azure-devices.net/MyDevice01`。

- 針對 [密碼] 欄位使用 SAS 權杖。SAS 權杖的格式與 HTTP 和 AMQP 通訊協定的格式相同：<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`。

    如需如何產生 SAS 權杖的詳細資訊，請參閱[使用 IoT 中樞安全性權杖][lnk-sas-tokens]的裝置一節。
    
    測試時，您也可以使用[裝置總管][lnk-device-explorer]工具，快速產生 SAS 權杖，讓您可以複製並貼到您的程式碼︰
    
    1. 移至裝置檔案總管中的 [管理] 索引標籤。
    2. 按一下 [SAS 權杖] \(右上角)。
    3. 在 [SASTokenForm] 的 [DeviceID] 下拉式清單中，選取您的裝置。設定您的 **TTL**。
    4. 按一下 [產生] 來建立您的權杖。
    
    產生的 SAS 權杖看起來如下：`HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`。

    此項目在 [密碼] 欄位中使用 MQTT 連線時所使用的部分是︰`SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`。

對於 MQTT 的連接和中斷連接封包，IoT 中樞會對**作業監視**通道發出事件。

### 將訊息傳送至 IoT 中樞

成功連接之後，裝置可以使用 `devices/{device_id}/messages/events/` 或 `devices/{device_id}/messages/events/{property_bag}` 作為「主題名稱」，將訊息傳送至 IoT 中樞。`{property_bag}` 項目可讓裝置以 URL 編碼格式傳送具有其他屬性的訊息。例如：

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] 這是與用於 HTTP 通訊協定中的查詢字串相同的編碼。

裝置用戶端應用程式也可以使用 `devices/{device_id}/messages/events/{property_bag}` 作為「Will 主題名稱」，來定義要以遙測訊息形式轉送的「Will 訊息」。

### 接收訊息

若要從 IoT 中樞接收訊息，裝置應該使用 `devices/{device_id}/messages/devicebound/#”` 作為「主題篩選」來進行訂閱。如果有任何訊息屬性，IoT 中樞會傳遞具有**主題名稱** `devices/{device_id}/messages/devicebound/` 或 `devices/{device_id}/messages/devicebound/{property_bag}` 的訊息。`{property_bag}` 包含以 URL 編碼的訊息屬性索引鍵/值組。屬性包中只會包含應用程式屬性和使用者可設定的系統屬性 (例如 **messageId** 或 **correlationId**)。系統屬性名稱具有前置詞 **$**，但應用程式屬性則會使用沒有前置詞的原始屬性名稱。

## 後續步驟

如需 IoT 裝置 SDK 的 MQTT 支援詳細資訊，請參閱《Azure IoT 中樞開發人員指南》中的 [MQTT 支援的注意事項][lnk-mqtt-devguide]。

若要深入了解 MQTT 通訊協定，請參閱 [MQTT 文件][lnk-mqtt-docs]。

若要深入了解如何規劃 IoT 中樞部署，請參閱：

- [支援的裝置][lnk-devices]
- [支援其他通訊協定][lnk-protocols]
- [與事件中樞比較][lnk-compare]
- [縮放、HA 及 DR][lnk-scaling]

若要進一步探索 IoT 中樞的功能，請參閱︰

- [開發人員指南][lnk-devguide]
- [使用範例 UI 探索裝置管理][lnk-dmui]
- [使用閘道 SDK 模擬裝置][lnk-gateway]
- [使用 Azure 入口網站管理 IoT 中樞][lnk-portal]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-sas-tokens.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide.md#mqtt-support

[lnk-devices]: iot-hub-tested-configurations.md
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0720_2016-->