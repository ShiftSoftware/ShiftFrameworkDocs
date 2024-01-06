The Shared Project is where you place the common classes that are used by most of the other projects. It is mostly used to keep data structures ([DTOs](/project-setup/shared-project/dtos/) and [Enums](/project-setup/shared-project/enums/)) available for other projects.

It is also where we keep the [Action Trees](/project-setup/shared-project/action-trees/) used in autherization.

## Project Dependencies

The Shared Project is not dependent on any other project. But almost all the other projects will reference it.
The follwoing outlines which projects reference the Shared Project:
```hl_lines="3 4 5 12 13"
StockPlusPlus
|
├─ StockPlusPlus.API		<--- (References Shared through Data)
├─ StockPlusPlus.Data		<--- (References Shared Directly)
├─ StockPlusPlus.Functions	<--- (References Shared through Data)
├─ StockPlusPlus.Shared
│   ├── ActionTrees
│   ├── Constants
│   ├── DTOs
│   ├── Enums
│
├─ StockPlusPlus.Test		<--- (References Shared through API or Data)
├─ StockPlusPlus.Web		<--- (References Shared Directly)
```

## Package Dependencies

The Shared project is kept clean and has very few package dependencies since it's referenced by many projects with different SDKs.

Below are the required Shift Framework packages.

```
ShiftSoftware.ShiftEntity.Model
```

```
ShiftSoftware.TypeAuth.Core
```

Available as nuget packages:
[**ShiftSoftware.ShiftEntity.Model**](https://www.nuget.org/packages/ShiftSoftware.ShiftEntity.Model)
and
[**ShiftSoftware.TypeAuth.Core**](https://www.nuget.org/packages/ShiftSoftware.TypeAuth.Core)

!!! warning
	Be considerate when installing new packages to the Shared project.
	Keep in mind that it's referenced by projects that're using different SDKs.

	* AspNet Core Web API
	* Azure Functions
	* Blazor WASM
	* Class Library