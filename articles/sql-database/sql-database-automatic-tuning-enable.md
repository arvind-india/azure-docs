---
title: Enable automatic tuning for Azure SQL Database | Microsoft Docs
description: You can enable automatic tuning on your Azure SQL Database easily.
services: sql-database
author: danimir 
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: vvasic 

---
# Enable automatic tuning

Azure SQL Database is an automatically managed data service that constantly monitors your queries and identifies the action that you can perform to improve performance of your workload. You can review recommendations and manually apply them, or let Azure SQL Database automatically apply corrective actions - this is known as **automatic tuning mode**. Automatic tuning can be enabled at the server or the database level.

## Enable automatic tuning on server
On the server level you can choose to inherit automatic tuning configuration from "Azure Defaults" or not to inherit the configuration. Azure defaults are FORCE_LAST_GOOD_PLAN is enabled, CREATE_INDEX is enabled, and DROP_INDEX is disabled.

### Azure portal
To enable automatic tuning on Azure SQL Database **server**, navigate to the server in Azure portal and then select **Automatic tuning** in the menu. Select the automatic tuning options you want to enable and select **Apply**.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Please note that **DROP_INDEX** option at this time is incompatible with applications using partition switching and index hints and should not be turned on in these cases.
>

Automatic tuning options on server are applied to all databases on the server. By default, all databases inherit the configuration from their parent server, but this can be overridden and specified for each database individually.

### REST API
[Click here, to read more about how to enable automatic tuning on the server level via REST API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## Enable automatic tuning on an individual database

The Azure SQL Database enables you to individually specify the automatic tuning configuration on each database. On the database level you can choose to inherit automatic tuning configuration from parent server, "Azure Defaults" or not to inherit the configuration. Azure Defaults are FORCE_LAST_GOOD_PLAN enabled, CREATE_INDEX enabled, and DROP_INDEX disabled.

> [!NOTE]
> The general recommendation is to manage the automatic tuning configuration at server level so the same configuration settings can be applied on every database automatically. Configure automatic tuning on an individual database if the database is different that others on the same server.
>

### Azure portal

To enable automatic tuning on a **single database**, navigate to the database in the Azure portal and then and select **Automatic tuning**. You can configure a single database to inherit the settings from the server by selecting the option or you can specify the configuration for a database individually.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Once you have selected appropriate configuration, click **Apply**.

Please note that DROP_INDEX option at this time is incompatible with applications using partition switching and index hints and should not be turned on in these cases.

### Rest API
[Click here to read more about how to enable automatic tuning on a single database via REST API](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning)

### T-SQL

To enable automatic tuning on a single database via T-SQL, connect to the database and execute the following query:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Setting automatic tuning to AUTO will apply Azure Defaults. Setting it to INHERIT, automatic tuning configuration will be inherited from the parent server. Choosing CUSTOM, you will need to manually configure automatic tuning.

To configure individual automatic tuning options via T-SQL, connect to the database and execute the query such as this one:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
Setting the individual tuning option to ON, will override any setting that database inherited and enable the tuning option. Setting it to OFF, will also override any setting that database inherited and disable the tuning option. Automatic tuning option, for which DEFAULT is specified, will inherit the configuration from the database level automatic tuning setting.  

## Disabled by the system
Automatic tuning is monitoring all the actions it takes on the database and in some cases it can determine that automatic tuning can't properly work on the database. In this situation, tuning option will be disabled by the system. In most cases this happens because Query Store is not enabled or it's in read-only state on a specific database.

## Configure automatic tuning e-mail notifications

See [Automatic tuning e-mail notifications](sql-database-automatic-tuning-email-notifications.md)

## Next steps
* Read the [Automatic tuning article](sql-database-automatic-tuning.md) to learn more about automatic tuning and how it can help you improve your performance.
* See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.
* See [Query Performance Insights](sql-database-query-performance.md) to learn about viewing the performance impact of your top queries.
