3. Create **Entities** directory, you will put your entities here
4. Add a new entity calss under *Entities/Product* that inherits from **ShiftEntity<Brand>** as follows

```C#

using ShiftSoftware.ShiftEntity.Core;
using ShiftSoftware.ShiftEntity.Model;

namespace StockPlusPlus.Data.Entities.Product;

public class Brand : ShiftEntity<Brand>
{
    public string Name { get; set; } = default!;
    public string? Description { get; set; }
    public string? Code { get; set; }
}

```
