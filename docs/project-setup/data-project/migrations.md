
```hl_lines="7"
StockPlusPlus
|
├─ StockPlusPlus.API
├─ StockPlusPlus.Data
│   ├── AutoMapperProfiles
│   ├── Entities
│   ├── Migrations
│   ├── ReplicationModels
│   ├── Repositories
│   ├── DB.cs
│ 
├─ StockPlusPlus.Functions
├─ StockPlusPlus.Shared
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web
```

The DB registration and the connection strings are in **API** or/and **Functions** Project. But we want the migrations to be inside the **Data Project**. 

For this we need to specify both projects when running migration commands.

=== "Package Manager"
    ```
    Add-Migration name -Project StockPlusPlus.Data -StartupProject StockPlusPlus.API

    Update-Database -Project StockPlusPlus.Data -StartupProject StockPlusPlus.API
    ```
=== ".NET CLI"
    ```
    dotnet ef migrations add name --project StockPlusPlus.Data --startup-project StockPlusPlus.API

    dotnet ef database update --project StockPlusPlus.Data --startup-project StockPlusPlus.API
    ```
