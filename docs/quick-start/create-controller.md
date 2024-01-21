1. Create Controllers directory and add **BrandController.cs** class
2. You can simply inherit from BaseController if you don't want to use ShiftIdentiy to secure your API
3. Make sure in pass the `BrandRepository` to the controller constructor.
3. Add a test method to your controller as follows:

```C#
[ApiController]
[Route("api/[controller]")]
public class BrandController : ControllerBase
{
    private readonly BrandRepository brandRepository;
    public BrandController(BrandRepository brandRepository)
    {
        this.brandRepository = brandRepository;
    }

    [AllowAnonymous]
    [HttpGet("test-insert-and-view")]
    public async Task<IActionResult> TestInsertAndView()
    {
        var createdBrand = await this.brandRepository.UpsertAsync(
            entity: new Brand(),
            dto: new BrandDTO { Name = "One" },
            actionType: ShiftSoftware.ShiftEntity.Core.ActionTypes.Insert,
            userId: null
        );

        this.brandRepository.Add(createdBrand);

        await this.brandRepository.SaveChangesAsync();

        return Ok(await this.brandRepository.ViewAsync(createdBrand!));
    }
}
```