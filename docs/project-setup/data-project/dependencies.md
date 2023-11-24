The Data Project is where you place the following:

 * DB Context
 * Entities
 * Migrations 
 * Repositories
 * Automapper Profiles
 * Optionally, if CosmosDB Replication is enabled, the **Replication Models** are also stored in the Data Project

## Project Dependencies

The Data Project is referencing the Shared Project only.
```hl_lines="3 12 14"
StockPlusPlus
|
├─ StockPlusPlus.API		<--- (References Data)
├─ StockPlusPlus.Data
│   ├── AutoMapperProfiles
│   ├── Entities
│   ├── Migrations
│   ├── ReplicationModels
│   ├── Repositories
│   ├── DB.cs
│ 
├─ StockPlusPlus.Functions	<--- (References Data)
├─ StockPlusPlus.Shared
├─ StockPlusPlus.Test		<--- (References Data Directly or through API)
├─ StockPlusPlus.Web
```

## Package Dependencies

The Data Project requires the following Shift Framework packages.  

```
ShiftSoftware.ShiftEntity.EFCore
```

```
ShiftSoftware.ShiftIdentity.Data
```

!!! warning
	**``ShiftSoftware.ShiftIdentity.Data``** is only required if Shift Identity is included in your app. If you're using an External Shift Identity then no need to install this package.
   
Available as nuget packages:   
[**ShiftSoftware.ShiftEntity.EFCore**](https://www.nuget.org/packages/ShiftSoftware.ShiftEntity.EFCore)
and
[**ShiftSoftware.ShiftIdentity.Data**](https://www.nuget.org/packages/ShiftSoftware.ShiftIdentity.Data)
