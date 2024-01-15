1. To make DTOs avaialable in Data project, add a reference to Shared project. To do this right-click StockPlusPlus.Data project and select `Add -> Project Reference` and from the dialog select **StockPlusPlus.Shared**

    ![Screen 1](/quick-start/images/Data-3.png)

2. Create **AutoMapperProfiles** directory
3. Add automapper profile class under *Entities/Product* as follows:

```C#

using AutoMapper;
using StockPlusPlus.Shared.DTOs.Product.Brand;

namespace StockPlusPlus.Data.AutoMapperProfiles.Product;

public class Brand : Profile
{
    public Brand()
    {
        CreateMap<Entities.Product.Brand, BrandDTO>().ReverseMap();
        CreateMap<Entities.Product.Brand, BrandListDTO>();
    }
}

```