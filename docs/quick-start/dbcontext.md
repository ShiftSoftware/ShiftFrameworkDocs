1. In Data project "StockPlusPlus.Data" create DB context class in root directory.
2. Make sure to inherit from **ShiftDbContext**
3. Declare the Brand DBSet as follows:

```C#

using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftEntity.EFCore;
using StockPlusPlus.Data.Entities.Product;

namespace StockPlusPlus.Data
{
    public class DB : ShiftDbContext
    {
        public DB(DbContextOptions option) : base(option)
        {
        }

        public DbSet<Brand> Brands { get; set; }
    }
}

```