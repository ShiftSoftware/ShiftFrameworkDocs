The Test project does not heavily depend on the Shift Framework. But we do provide some basic tools.

## Project Dependencies

The test project is referencing the API Project. 
```hl_lines="3 4"
StockPlusPlus
|
├─ StockPlusPlus.API		<--- (Referenced by Test Directly)
├─ StockPlusPlus.Data		<--- (Referenced by Test via API Project)
├─ StockPlusPlus.Functions
├─ StockPlusPlus.Shared
├─ StockPlusPlus.Test
│   ├── Tests
│   ├── appsettings.json
│   ├── CustomWebApplicationFactory.cs
│   ├── Using.cs
│ 
├─ StockPlusPlus.Web
```

## Package Dependencies

The Test project is using the below package from Shift Framework.  

```
ShiftSoftware.ShiftFrameworkTestingTools
```

```
xunit.runner.visualstudio
```
   
Available as nuget packages:   
[**ShiftSoftware.ShiftFrameworkTestingTools**](https://www.nuget.org/packages/ShiftSoftware.ShiftFrameworkTestingTools)

!!! warning
	``ShiftSoftware.ShiftFrameworkTestingTools`` uses ``xunit`` package internally. Make sure you use the same version of ``xunit`` in your test project.