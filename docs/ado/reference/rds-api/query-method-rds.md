---
description: "Query Method (RDS)"
title: "Query Method (RDS) | Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ""
ms.date: "01/19/2017"
ms.reviewer: ""
ms.topic: reference
apitype: "COM"
helpviewer_keywords: 
  - "Query method [ADO]"
ms.assetid: 20f2480f-3758-405d-a379-05a0dce74796
author: rothja
ms.author: jroth
---
# Query Method (RDS)
Uses a valid SQL query string to return a [Recordset](../ado-api/recordset-object-ado.md).  
  
> [!IMPORTANT]
>  Beginning with Windows 8 and Windows Server 2012, RDS server components are no longer included in the Windows operating system (see Windows 8 and [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) for more detail). RDS client components will be removed in a future version of Windows. Avoid using this feature in new development work, and plan to modify applications that currently use this feature. Applications that use RDS should migrate to [WCF Data Service](/dotnet/framework/wcf/).  
  
## Syntax  
  
```  
  
Set Recordset = DataFactory.Query(Connection, Query)  
```  
  
#### Parameters  
 *Recordset*  
 An object variable that represents a **Recordset** object.  
  
 *DataFactory*  
 An object variable that represents an [RDSServer.DataFactory](./datafactory-object-rdsserver.md) object.  
  
 *Connection*  
 A **String** value that contains the server connection information. This is similar to the [Connect](./connect-property-rds.md) property.  
  
 *Query*  
 A **String** that contains the SQL query.  
  
## Remarks  
 The query should use the SQL dialect of the database server. A result status is returned if there is an error with the query that was executed. The **Query** method does not perform any syntax checking on the **Query** string.  
  
## Applies To  
 [DataFactory Object (RDSServer)](./datafactory-object-rdsserver.md)  
  
## See Also  
 [DataFactory Object, Query Method, and CreateObject Method Example (VBScript)](./datafactory-object-query-method-and-createobject-method-example-vbscript.md)