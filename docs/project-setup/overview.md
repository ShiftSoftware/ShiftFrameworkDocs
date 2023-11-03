A typical solution built with Shift Framework consists of 6 projects.  
Below is a sample solution named **StockPlusPlus**
```
StockPlusPlus
├─ StockPlusPlus.API		<--- (.NET 7 AspNet Core Web API)
├─ StockPlusPlus.Data		<--- (.NET 6 Class Library)
├─ StockPlusPlus.Functions	<--- (.NET 6 Azure Functions)
├─ StockPlusPlus.Shared		<--- (.NET 6 Class Library)
├─ StockPlusPlus.Test		<--- (.NET 6 Class Library)
├─ StockPlusPlus.Web		<--- (.NET 7 Blazor WASM)
```

Below is an overal view inside the solution projects.  
Some default folders and items like (Dependencies, Properties) are not shown for similicity.

```
StockPlusPlus
|
├─ StockPlusPlus.API
│   ├── Controllers
│   ├── appsettings.json
│   ├── Program.cs
│ 
├─ StockPlusPlus.Data
│   ├── AutoMapperProfiles
│   ├── Entities
│   ├── Migrations
│   ├── Repositories
│   ├── DB.cs
│ 
├─ StockPlusPlus.Functions
│   ├── Functions
│   ├── host.json
│   ├── local.settings.json
│   ├── Startup.cs
│ 
├─ StockPlusPlus.Shared
│   ├── Constants
│   ├── DTOs
│   ├── Enums
│   ├── ReplicationModels
│ 
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web
│   ├── wwwroot
│   ├── Pages
│   ├── Shared
│   ├── _Imports.razor
│   ├── App.razor
│   ├── Program.cs
│   ├── staticwebapp.config.json
```