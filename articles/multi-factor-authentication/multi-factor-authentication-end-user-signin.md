<properties 
	pageTitle="Azure Multi-Factor Authentication 的 Azure MFA 登入體驗" 
	description="本頁面將指引您移至何處才能看到 Azure MFA 可用的各種登入方法。"
	keywords="使用者驗證, 登入經驗, 使用行動電話登入, 使用辦公室電話登入" 
	services="multi-factor-authentication" 
	documentationCenter="" 
	authors="billmath" 
	manager="stevenpo" 
	editor="curtland"/>

<tags 
	ms.service="multi-factor-authentication" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/04/2016" 
	ms.author="billmath"/>

# Azure Multi-Factor Authentication 的登入體驗
> [AZURE.NOTE]  以下文件顯示一般登入體驗。如需登入的說明，請參閱[使用 Azure Multi-Factor Authentication 時碰到困難](multi-factor-authentication-end-user-manage-settings.md)



## 您的登入體驗將會如何？
視您登入及使用 Multi-Factor Authentication 的方式而定，您的體驗將會有所不同。本節將提供登入時預期情況的資訊。選擇最符合您的情況的一項：


您在做什麼？|說明
:------------- | :------------- | 
<a href="#-1">使用行動或辦公室電話登入</a> | 這是使用行動或辦公室電話登入時所能預期的情況。
<a href="#-3">使用通知登入行動應用程式</a> | 這是使用通知登入行動應用程式時所能預期的情況。
<a href="#-5">使用驗證碼登入行動應用程式</a> | 這是使用驗證碼登入行動應用程式時所能預期的情況。
<a href="#-7">使用替代方法登入</a> | 這會顯示想要使用替代方法時所能預期的情況。

<h2 id="-1">使用行動或辦公室電話登入</h2>

下列資訊將說明搭配行動電話或辦公室電話使用 Multi-Factor Authentication 的經驗。

### 透過辦公室或行動電話進行登入

- 使用您的使用者名稱和密碼登入應用程式或服務，例如 Office 365。
- Microsoft 將會打電話給您。

![Microsoft 來電](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- 請接聽電話並按 # 鍵。

![Answer](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- 您現在應已登入。</li>

<h2 id="-3">使用通知登入行動應用程式</h2>

下列資訊將說明當您接獲通知時，將 Multi-Factor Authentication 使用於您的行動應用程式的經驗。

### 使用傳送至您的行動應用程式的通知登入

- 使用您的使用者名稱和密碼登入應用程式或服務，例如 Office 365。
- Microsoft 會傳送通知。

![Microsoft 傳送通知](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- 請接聽電話並按驗證鍵。

![Verify](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)


- 您現在應已登入。


<h2 id="-5">使用驗證碼登入行動應用程式</h2>

下列資訊將說明當您搭配驗證碼使用 Multi-Factor Authentication 時，將 Multi-Factor Authentication 使用於您的行動應用程式的經驗。

### 搭配行動應用程式使用驗證碼進行登入

- 使用您的使用者名稱和密碼登入應用程式或服務，例如 Office 365。
- Microsoft 將會提示您輸入驗證碼。

![輸入驗證碼](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- 在電話上開啟 Azure Authenticatior 應用程式，並在登入時的方塊中輸入驗證碼。

![取得驗證碼](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)

- 您現在應已登入。


<h2 id="-7">使用替代方法登入</h2>


下一節將說明當主要方法無法使用時，如何使用替代方法進行登入。

### 使用替代方法進行登入

- 使用您的使用者名稱和密碼登入應用程式或服務，例如 Office 365。
- 選取使用不同的驗證選項。您將會看見各種不同的選項。您所看到的個數將以您設定的個數為基礎。

![使用替代方法](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- 選擇替代方法並且登入。

 

<!---HONumber=AcomDC_0810_2016------>