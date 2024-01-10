1. Under **StockPlusPlus.Shared** project Create **DTOs** directory, you will put your DTOs here
2. Add a view and update (Upsert) DTO class **BrandDTO** under *DTOs/Product/Brand*
3. Upsert DTO should inherit from **ShiftEntityViewAndUpsertDTO** and implement the property ID
4. Alonng with ID, you can add the properties that will be avaialable for updating and viewing from the **Brand** entity

```C#

using ShiftSoftware.ShiftEntity.Model.Dtos;
using System.ComponentModel.DataAnnotations;

namespace StockPlusPlus.Shared.DTOs.Product.Brand
{
    public class BrandDTO : ShiftEntityViewAndUpsertDTO
    {
        public override string? ID { get; set; }
        [Required]
        public string Name { get; set; } = default!;
        public string? Description { get; set; }
        public string? Code { get; set; }

    }
}

```
5. At the same directory add listing DTO **BrandListDTO**
6. The listing DTO should inherit from **ShiftEntityListDTO** and also needs to implemnt the field ID

```C#

using ShiftSoftware.ShiftEntity.Model.Dtos;

namespace StockPlusPlus.Shared.DTOs.Product.Brand
{
    public class BrandListDTO : ShiftEntityListDTO
    {
        public override string? ID { get; set; }
        public string Name { get; set; } = default!;
        public string? Description { get; set; }
        public string? Code { get; set; }

    }
}

```