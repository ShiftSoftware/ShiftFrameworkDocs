The ``ShiftSoftware.ShiftFrameworkTestingTools`` package provides a custom WebApplicationFactory called ``ShiftCustomWebApplicationFactory<TStartup, DB>`` that can be used for integration testing against the API project.

## ShiftCustomWebApplicationFactory&lt;WebMarker, DB&gt;

```C#
public class CustomWebApplicationFactory :
    ShiftCustomWebApplicationFactory<WebMarker, DB>
{
    public CustomWebApplicationFactory() : base(
        "SQLServer_Test",
        new ShiftCustomWebApplicationBearerAuthSettings
        {
            Enabled = true,
            TokenKeySettingKey = "Settings:TokenSettings:Key",
            TokenIssuerSettingKey = "Settings:TokenSettings:Issuer",
            TypeAuthActions = new List<Type>()
            {
                typeof(StockActionTrees)
            }
        })
    { }
}
```

## App Settings

The constructor reads from appsettings. Below is a sample **``appsettings.json``** that should exist in the test project.

```JSON
{
  "ConnectionStrings": {
    "SQLServer_Test": "Server=localhost\\sqlexpress;Initial Catalog=StockPlusPlus_Test;Persist Security Info=True;Integrated Security=SSPI;TrustServerCertificate=True;"
  }
}
```
