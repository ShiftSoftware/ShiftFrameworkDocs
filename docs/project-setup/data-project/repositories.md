Shift Framework provides a default repository implementation called **``ShiftRepository``**.   
The **``ShiftRepository``** seamlessly integrates with the other parts of the Framework.

```hl_lines="9"
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

## Shift Repository
A simple repository can be created by inheriting from   
**``ShiftRepository<DB, Entity, ListDTO, ViewAndUpsertDTO>``**

```C#
using ShiftSoftware.ShiftEntity.EFCore;
using StockPlusPlus.Shared.DTOs.Product.Brand;

public class BrandRepository : 
    ShiftRepository<DB, Entities.Product.Brand, BrandListDTO, BrandDTO>
{
    public BrandRepository(DB db) : base(db)
    {
    }
}
```


## Default ShiftRepository Functions

!!! note
    If you're using **``ShiftEntity.Web``**, the default controller provides implementation for CRUD operations and data listing. 
    In this case you don't need to manualy invoke the default repository functions.

But in case you do need to call the default repository functions. Below are some examples:

### Create
```C#
var createdBrand = await this.brandRepository.UpsertAsync(
    entity: new Brand(),
    dto: new BrandDTO { Name = "One" },
    actionType: ShiftSoftware.ShiftEntity.Core.ActionTypes.Insert,
    userId: null
);

this.brandRepository.Add(createdBrand);

await this.brandRepository.SaveChangesAsync();
```

### Find
```C#
var brandEntity = await this.brandRepository.FindAsync(1);
```

### View
```C#
var brandDto = await this.brandRepository.ViewAsync(
    await this.brandRepository.FindAsync(1)
);
```

### Update
```C#
var updatedBrand = await this.brandRepository.UpsertAsync(
    entity: await this.brandRepository.FindAsync(1), 
    dto: new BrandDTO { ID = "1", Name = "Updated" }, 
    actionType: ShiftSoftware.ShiftEntity.Core.ActionTypes.Update, 
    userId: null
);

await this.brandRepository.SaveChangesAsync();
```

### Delete
```C#
var deletedBrand = await this.brandRepository.DeleteAsync(
    entity: await this.brandRepository.FindAsync(1),
    isHardDelete: false,
    userId: null
);

await this.brandRepository.SaveChangesAsync();
```

## Overriding Default Functions
All the default functions in the **``ShiftRepository``** are virtual and can be overridden.  

```C#
using AutoMapper;
using ShiftSoftware.ShiftEntity.EFCore;
using StockPlusPlus.Shared.DTOs.Product.Brand;

public class BrandRepository : 
    ShiftRepository<DB, Entities.Product.Brand, BrandListDTO, BrandDTO>
{
    public BrandRepository(DB db, IMapper mapper) : base(db, db.Brands, mapper)
    {
    }

    public override async ValueTask<Brand> UpsertAsync(Brand entity, BrandDTO dto, ActionTypes actionType, long? userId = null)
    {
        //Do something here

        return await base.UpsertAsync(entity, dto, actionType, userId);
    }

    public override ValueTask<BrandDTO> ViewAsync(Brand entity)
    {
        //Do something here
        return base.ViewAsync(entity);
    }
}
```

## Include Related Entities
The base constructor of the ShiftRepository accepts an action for configuring **``ShiftRepositoryOptions``**.   

The **``IncludeRelatedEntitiesWithFindAsync``** method can be used to include one or more related entities.

```C#
public ProductRepository(DB db) :base(db, 
    x => x.IncludeRelatedEntitiesWithFindAsync(
        y => y.Include(z => z.Brand),
        y => y.Include(z => z.ProductCategory)
    ))
{
}
```

!!! note
    When related entities are included, the framework makes an additional roundtrip to the database by calling **``FindAsync``** after every **``SaveChangeAsync``**.  

    This ensures that when a entities are added or updated, their related entities are included with the returned DTO.