##Add Database Connection String
1. First you define the connection string. The connection string will be stored in the API project. Open the file **appsettings.json** from StockPlusPlus.API and add the follwing section to it:
```JSON
"ConnectionStrings": {
    "SQLServer": "Server=localhost\\sqlexpress;Initial Catalog=StockPlusPlus;Persist Security Info=True;Integrated Security=SSPI;TrustServerCertificate=True;"
  },
```

##Configure Enitiy Framework
2. Add the following lines in Program.cs
```C#
Action<DbContextOptionsBuilder> dbOptionBuilder = x =>
{
    x.UseSqlServer(builder.Configuration.GetConnectionString("SQLServer")!)
};
builder.Services.AddDbContext<DB>(dbOptionBuilder);
```

