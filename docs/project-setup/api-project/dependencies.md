The API Project is the AspNet Core Web API

## Project Dependencies

The API Project is referencing the Data Project & Web Project.
```hl_lines="9 11 13"
StockPlusPlus
|
├─ StockPlusPlus.API		
│   ├── Controllers
│   ├── appsettings.json
│   ├── Program.cs
│   ├── WebMarker.cs
│ 
├─ StockPlusPlus.Data		<--- (Referenced by API Directly)
├─ StockPlusPlus.Functions	
├─ StockPlusPlus.Shared		<--- (Referenced by API via Data)
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web		<--- (Referenced by API Directly)
```

## Package Dependencies

The API Project requires the following Shift Framework Packages.  

```
ShiftSoftware.ShiftEntity.Web
```

```
ShiftSoftware.ShiftIdentity.Dashboard.AspNetCore
```

!!! warning
	**``ShiftSoftware.ShiftIdentity.Dashboard.AspNetCore``** is only required if Shift Identity is included in your app. If you're using an External Shift Identity then no need to install this package.
   
Available as nuget packages:   
[**ShiftSoftware.ShiftEntity.Web**](https://www.nuget.org/packages/ShiftSoftware.ShiftEntity.Web)
and
[**ShiftSoftware.ShiftIdentity.Dashboard.AspNetCore**](https://www.nuget.org/packages/ShiftSoftware.ShiftIdentity.Dashboard.AspNetCore)
