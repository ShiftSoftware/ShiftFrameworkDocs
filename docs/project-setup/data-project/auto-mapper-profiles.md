Shift Framework is using [AutoMapper](https://www.nuget.org/packages/automapper) to map from and to Entities.

```hl_lines="5 6"
StockPlusPlus
|
├─ StockPlusPlus.API
├─ StockPlusPlus.Data
│   ├── AutoMapperProfiles
│		├── ProductCategory.cs
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

A simple Mapping Profile looks like this:

```C#
public class ProductCategory : Profile
{
    public ProductCategory()
    {
        //Maps Entity to DTO. Then DTO to Entity by calling ReverseMap()
        CreateMap<Entities.Product.ProductCategory, ProductCategoryDTO>()
            .ReverseMap();

        CreateMap<Entities.Product.ProductCategory, ProductCategoryListDTO>();
    }
}
```
