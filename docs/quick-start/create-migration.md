1. Make sure that you have defined the database connection and configured EF in API project
2. Run the following commands:
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

!!! note
    If the Add-Migration failed try installing **Microsoft.EntityFrameworkCore.Tools** by running the commnd in Package Manager:

    ```Install-Package Microsoft.EntityFrameworkCore.Tools -Version 6.0.20```

