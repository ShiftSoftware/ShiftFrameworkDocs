1. In Data project "StockPlusPlus.Data" create directory **Repositories**.
2. Create BrandRepository.cs calss under *Repositories/Product*
3. The repository class should inherits from
**`ShiftRepository<DB, Entity, ListDTO, ViewAndUpsertDTO>`**
4. The repository should look like this:
```C#

using ShiftSoftware.ShiftEntity.EFCore;
using StockPlusPlus.Data.Entities.Product;
using StockPlusPlus.Shared.DTOs.Product.Brand;

namespace StockPlusPlus.Data.Repositories.Product;

public class BrandRepository : ShiftRepository<DB, Entities.Product.Brand, BrandListDTO, BrandDTO>
{
    public BrandRepository(DB db) : base(db)
    {
    }
}

```
5. In case you wanted to override any of the default methods you can define an override method as in the following example:
```C#
public override IQueryable<BrandListDTO> OdataList(bool showDeletedRows = false, IQueryable<Brand>? queryable = null)
{
    return base.OdataList(showDeletedRows, queryable);
}
```