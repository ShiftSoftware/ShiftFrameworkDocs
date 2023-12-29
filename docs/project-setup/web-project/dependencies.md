The Web Project is the .NET 7 Blazor WASM Project.

## Project Dependencies

The Web Project is referencing the Shared Project
```hl_lines="6"
StockPlusPlus
|
├─ StockPlusPlus.API		
├─ StockPlusPlus.Data
├─ StockPlusPlus.Functions	
├─ StockPlusPlus.Shared		<--- (Referenced by Web Directly)
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web
```

## Package Dependencies

The Web Project requires the following Shift Framework Packages.  

```
ShiftSoftware.ShiftBlazor
```

```
ShiftSoftware.ShiftIdentity.Blazor
```

```
ShiftSoftware.ShiftIdentity.Dashboard.Blazor
```

!!! warning
	**``ShiftSoftware.ShiftIdentity.Dashboard.Blazor``** is only required if Shift Identity is included in your app. If you're using an External Shift Identity then no need to install this package.
   
Available as nuget packages:   
[**ShiftSoftware.ShiftBlazor**](https://www.nuget.org/packages/ShiftSoftware.ShiftBlazor)   

[**ShiftSoftware.ShiftIdentity.Blazor**](https://www.nuget.org/packages/ShiftSoftware.ShiftIdentity.Blazor)  

[**ShiftSoftware.ShiftIdentity.Dashboard.Blazor**](https://www.nuget.org/packages/ShiftSoftware.ShiftIdentity.Dashboard.Blazor)
