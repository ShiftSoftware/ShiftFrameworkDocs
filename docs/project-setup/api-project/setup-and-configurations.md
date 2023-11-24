# Setup, Configurations & Dependency Injection

## EF Core & Database

``` C#
Action<DbContextOptionsBuilder> dbOptionBuilder = x =>
{
    x.UseSqlServer("Server=localhost\\sqlexpress;Initial Catalog=StockPlusPlus;Persist Security Info=True;Integrated Security=SSPI;TrustServerCertificate=True;")
    .UseTemporal(true);
};

builder.Services.AddDbContext<DB>(dbOptionBuilder);
```

## Register Repositories
You can register the repositories one by one. Or use the `RegisterShiftRepositories` extension method.
``` C#
builder.Services.RegisterShiftRepositories(
    typeof(StockPlusPlus.Data.Marker).Assembly
);
```

## TypeAuth & Action Trees
Enable TypeAuth and Register the Action Trees
``` C#
builder.Services.AddTypeAuth((o) =>
{
    o.AddActionTree<ShiftIdentityActions>();
    o.AddActionTree<StockPlusPlus.Shared.ActionTrees.SystemActionTrees>();
    o.AddActionTree<StockPlusPlus.Shared.ActionTrees.StockActionTrees>();
});
```

## Shift Entity Web
Enables the Shift Entity and provides an option for configuring it.
``` C#
mvcBuilder.AddShiftEntityWeb(x =>
{
    x.WrapValidationErrorResponseWithShiftEntityResponse(true);

    x.AddAutoMapper(typeof(StockPlusPlus.Data.Marker).Assembly);

    x.HashId.RegisterHashId(false);

    x.HashId.RegisterIdentityHashId("one-two", 5);

    x.AddShiftIdentityAutoMapper();
});
```

## Odata
Enable and Configure oData.
``` C#
mvcBuilder.AddShiftEntityOdata(x =>
{
    x.DefaultOptions();
    x.RegisterAllDTOs(typeof(StockPlusPlus.Shared.Marker).Assembly);
    x.RegisterShiftIdentityDashboardEntitySets();
});
```
The `RegisterAllDTOs` registers all oData endpoints by convention for all [Listing DTOs](/project-setup/shared-project/dtos/#listing-dto).   
You can also register them one by one if you want.

``` C# hl_lines="5-10"
mvcBuilder.AddShiftEntityOdata(x =>
{
    x.DefaultOptions();

    //enables odata/Product endpoint
    x.OdataEntitySet<ProductListDTO>("Product");

    //enables odata/ProductCategory endpoint
    x.OdataEntitySet<ProductCategoryListDTO>("ProductCategory");
    //...etc

    x.RegisterShiftIdentityDashboardEntitySets();
});
```

## Shift Identity
Enables the Shift Identity by providing an Issuer and a Key.
``` C#
mvcBuilder.AddShiftIdentity(
    "StockPlusPlus", 
    "one-two-three-four-five-six-seven-eight.one-two-three-four-five-six-seven-eight"
);
```

## Shift Identity Dashboard
If your app is hosting Shift Identity Internally, below will add the dashboard.
``` C#
mvcBuilder.AddShiftIdentityDashboard<DB>(
    new ShiftIdentityConfiguration
    {
        ShiftIdentityHostingType = ShiftIdentityHostingTypes.Internal,
        Token = new TokenSettingsModel
        {
            ExpireSeconds = 60000,
            Issuer = "StockPlusPlus",
            Key = "one-two-three-four-five-six-seven-eight.one-two-three-four-five-six-seven-eight",
        },
        Security = new SecuritySettingsModel
        {
            LockDownInMinutes = 0,
            LoginAttemptsForLockDown = 1000000,
            RequirePasswordChange = false
        },
        RefreshToken = new TokenSettingsModel
        {
            Audience = "stock-plus-plus",
            ExpireSeconds = 60000000,
            Issuer = "StockPlusPlus",
            Key = "one-two-three-four-five-six-seven-eight.one-two-three-four-five-six-seven-eight",
        },
        HashIdSettings = new HashIdSettings
        {
            AcceptUnencodedIds = true,
            UserIdsSalt = "k02iUHSb2ier9fiui02349AbfJEI",
            UserIdsMinHashLength = 5
        },
    }
);
```

The Shift Identity Dashboard provides a Seed method to configure the defaults
``` C#
await app.SeedDBAsync(
    "SuperUser",
    "OneTwo",
    new ShiftSoftware.ShiftIdentity.Data.DBSeedOptions
    {
        RegionExternalId = "1",
        RegionShortCode = "KRG",

        CompanyShortCode = "SFT",
        CompanyExternalId = "-1",
        CompanyAlternativeExternalId = "shift-software",
        CompanyType = ShiftSoftware.ShiftIdentity.Core.Enums.CompanyTypes.NotSpecified,

        CompanyBranchExternalId = "-11",
        CompanyBranchShortCode = "SFT-EBL"
    }
);
```