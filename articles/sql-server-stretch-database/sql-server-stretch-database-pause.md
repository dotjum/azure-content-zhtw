<properties
	pageTitle="暫停和繼續資料移轉 (Stretch Database) | Microsoft Azure"
	description="了解如何暫停或繼續資料移轉至 Azure。"
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="douglasl"/>

# 暫停和繼續資料移轉 (Stretch Database)

若要暫停或繼續將資料移轉至 Azure，請在 SQL Server Management Studio 中針對資料表選取 [Stretch]，然後選取 [暫停] 來暫停資料移轉，或選擇 [繼續] 來繼續資料移轉。您也可以使用 Transact-SQL 來暫停或繼續資料移轉。

若您想在本機伺服器上針對問題疑難排解，或想將可用的網路頻寬最大化時，可在個別資料表上暫停資料移轉。

## 暫停資料移轉

### 使用 SQL Server Management Studio 暫停資料移轉

1.  在 SQL Server Management Studio 中，請選取 [物件總管] 中想要暫停資料移轉的 Stretch 資料表。

2.  以滑鼠右鍵按一下並選取 [Stretch]，然後選取 [暫停]。

### 使用 Transact-SQL 暫停資料移轉
執行下列命令。

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## 繼續資料移轉

### 使用 SQL Server Management Studio 繼續資料移轉

1.  在 SQL Server Management Studio 中，請選取 [物件總管] 中想要繼續資料移轉的 Stretch 資料表。

2.  以滑鼠右鍵按一下並選取 [Stretch]，然後選取 [繼續]。

### 使用 Transact-SQL 繼續資料移轉
執行下列命令。

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## 查看移轉是否為作用中或已暫停

### 使用 SQL Server Management Studio 查看移轉是否為作用中或已暫停
在 SQL Server Management Studio 中，開啟 [Stretch Database 監視器]，然後查看 [移轉狀態] 欄中的值。如需詳細資訊，請參閱[資料移轉的監視及疑難排解](sql-server-stretch-database-monitor.md)。

### 使用 Transact-SQL 查看移轉是否為作用中或已暫停
查詢目錄檢視 **sys.remote\_data\_archive\_tables**，並查看 **is\_migration\_paused** 欄的值。如需詳細資訊，請參閱 [sys.remote\_data\_archive\_tables](https://msdn.microsoft.com/library/dn935003.aspx)。

## 另請參閱

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx) [資料移轉的監視及疑難排解](sql-server-stretch-database-monitor.md)

<!---HONumber=AcomDC_0622_2016-->