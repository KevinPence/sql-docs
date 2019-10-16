---
title: "Database mirroring in SQL Server"
description: "Describes database mirroring functionality."
ms.date: "09/30/2019"
dev_langs: 
  - "csharp"
ms.assetid: 89befaff-bb46-4290-8382-e67cdb0e3de9
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: v-kaywon
ms.author: v-kaywon
ms.reviewer: rothja
---
# Database mirroring in SQL Server

![Download-DownArrow-Circled](../../../ssdt/media/download.png)[Download ADO.NET](../../sql-connection-libraries.md#anchor-20-drivers-relational-access)

Database mirroring in SQL Server allows you to keep a copy, or mirror, of a SQL Server database on a standby server. Mirroring ensures that two separate copies of the data exist at all times, providing high availability and complete data redundancy. The Microsoft SqlClient Provider for SQL Server provides implicit support for database mirroring, so that the developer does not need to take any action or write any code once it has been configured for a SQL Server database. In addition, the <xref:Microsoft.Data.SqlClient.SqlConnection> object supports an explicit connection mode that allows supplying the name of a failover partner server in the <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.  
  
The following simplified sequence of events occurs for a <xref:Microsoft.Data.SqlClient.SqlConnection> object that targets a database configured for mirroring:  
  
1. The client application successfully connects to the principal database, and the server sends back the name of the partner server, which is then cached on the client.  
  
2. If the server containing the principal database fails or connectivity is interrupted, connection and transaction state is lost. The client application attempts to re-establish a connection to the principal database and fails.  
  
3. The client application then transparently attempts to establish a connection to the mirror database on the partner server. If it succeeds, the connection is redirected to the mirror database, which then becomes the new principal database.  
  
## Specifying the failover partner in the connection string  
If you supply the name of a failover partner server in the connection string, the client will transparently attempt a connection with the failover partner if the principal database is unavailable when the client application first connects.  
  
```csharp
";Failover Partner=PartnerServerName"  
```  
  
If you omit the name of the failover partner server and the principal database is unavailable when the client application first connects then a <xref:Microsoft.Data.SqlClient.SqlException> is raised.  
  
When a <xref:Microsoft.Data.SqlClient.SqlConnection> is successfully opened, the failover partner name is returned by the server and supersedes any values supplied in the connection string.  
  
> [!NOTE]
>  You must explicitly specify the initial catalog or database name in the connection string for database mirroring scenarios. If the client receives failover information on a connection that doesn't have an explicitly specified initial catalog or database, the failover information is not cached and the application does not attempt to fail over if the principal server fails. If a connection string has a value for the failover partner, but no value for the initial catalog or database, an `InvalidArgumentException` is raised.  
  
## Retrieving the current server name  
In the event of a failover, you can retrieve the name of the server to which the current connection is actually connected by using the <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> property of a <xref:Microsoft.Data.SqlClient.SqlConnection> object. The following code fragment retrieves the name of the active server, assuming that the connection variable references an open <xref:Microsoft.Data.SqlClient.SqlConnection>.  
  
When a failover event occurs and the connection is switched to the mirror server, the **DataSource** property is updated to reflect the mirror name.  
  
```csharp  
string activeServer = connection.DataSource;  
```  
  
## SqlClient mirroring behavior  
The client always tries to connect to the current principal server. If it fails, it tries the failover partner. If the mirror database has already been switched to the principal role on the partner server, the connection succeeds and the new principal-mirror mapping is sent to the client and cached for the lifetime of the calling <xref:System.AppDomain>. It is not stored in persistent storage and is not available for subsequent connections in a different **AppDomain** or process. However, it is available for subsequent connections within the same **AppDomain**. Note that another **AppDomain** or process running on the same or a different computer always has its pool of connections, and those connections are not reset. In that case, if the primary database goes down, each process or **AppDomain** fails once, and the pool is automatically cleared.  
  
> [!NOTE]
>  Mirroring support on the server is configured on a per-database basis. If data manipulation operations are executed against other databases not included in the principal/mirror set, either by using multipart names or by changing the current database, the changes to these other databases do not propagate in the event of failure. No error is generated when data is modified in a database that is not mirrored. The developer must evaluate the possible impact of such operations.  
  
## Next steps
### Database mirroring resources  
For conceptual documentation and information on configuring, deploying and administering mirroring, see the following resources in SQL Server documentation.  
  
|Resource|Description|  
|--------------|-----------------|  
|[Database Mirroring](../../../database-engine/database-mirroring/database-mirroring-sql-server.md)|Describes how to set up and configure mirroring in SQL Server.|  