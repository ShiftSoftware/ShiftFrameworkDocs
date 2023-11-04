Depending on your Shift Identity setup, The following DB Contexts are available: **``ShiftDbContext``** and **``ShiftIdentityDbContext``**.

```hl_lines="11"
StockPlusPlus
|
├─ StockPlusPlus.API
├─ StockPlusPlus.Data
│   ├── AutoMapperProfiles
│   ├── Entities
│       ├── Brand.cs
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

## ShiftDbContext 
Use this if the app is connected to an external Shift Identity.  
In this case your database does not need to include the identity related entites (Users, Companies ...etc).

```C#
public class DB : ShiftDbContext
{
    public DB(DbContextOptions option) : base(option)
    {
    }

    public DbSet<Brand> Brands { get; set; }
    public DbSet<ProductCategory> ProductCategories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```

## ShiftIdentityDbContext 
Use this if the Identity App is required to be included within your app.  
In this case your database needs to include the identity related entites (Users, Companies ...etc).

```C#
public class DB : ShiftIdentityDbContext
{
    public DB(DbContextOptions option) : base(option)
    {
    }

    public DbSet<Brand> Brands { get; set; }
    public DbSet<ProductCategory> ProductCategories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```
