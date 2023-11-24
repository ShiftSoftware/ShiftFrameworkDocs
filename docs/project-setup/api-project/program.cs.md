``` C#
using Microsoft.EntityFrameworkCore;
using ShiftSoftware.ShiftIdentity.AspNetCore.Models;
using ShiftSoftware.ShiftIdentity.AspNetCore;
using ShiftSoftware.ShiftIdentity.Core;
using StockPlusPlus.Data;
using ShiftSoftware.ShiftIdentity.Dashboard.AspNetCore.Extentsions;
using ShiftSoftware.ShiftIdentity.AspNetCore.Extensions;
using ShiftSoftware.TypeAuth.AspNetCore.Extensions;

var builder = WebApplication.CreateBuilder(args);

Action<DbContextOptionsBuilder> dbOptionBuilder = x =>
{
    x.UseSqlServer("Server=localhost\\sqlexpress;Initial Catalog=StockPlusPlus;Persist Security Info=True;Integrated Security=SSPI;TrustServerCertificate=True;")
    .UseTemporal(true);
};

builder.Services.AddDbContext<DB>(dbOptionBuilder);

builder.Services.RegisterShiftRepositories(typeof(StockPlusPlus.Data.Marker).Assembly);

builder.Services.AddTypeAuth((o) =>
{
    o.AddActionTree<ShiftIdentityActions>();
    o.AddActionTree<StockPlusPlus.Shared.ActionTrees.SystemActionTrees>();
    o.AddActionTree<StockPlusPlus.Shared.ActionTrees.StockActionTrees>();
});

#if DEBUG
builder.Services.AddRazorPages();
#endif

var mvcBuilder = builder.Services
    .AddLocalization()
    .AddHttpContextAccessor()
    .AddControllers();

mvcBuilder.AddShiftEntityWeb(x =>
{
    x.WrapValidationErrorResponseWithShiftEntityResponse(true);

    x.AddAutoMapper(typeof(StockPlusPlus.Data.Marker).Assembly);

    x.HashId.RegisterHashId(false);

    x.HashId.RegisterIdentityHashId("one-two", 5);

    x.AddShiftIdentityAutoMapper();
});

mvcBuilder.AddShiftEntityOdata(x =>
{
    x.DefaultOptions();

    x.RegisterAllDTOs(typeof(StockPlusPlus.Shared.Marker).Assembly);
    x.RegisterShiftIdentityDashboardEntitySets();
});

mvcBuilder.AddShiftIdentity("StockPlusPlus", "one-two-three-four-five-six-seven-eight.one-two-three-four-five-six-seven-eight");

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

var app = builder.Build();

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


#if DEBUG
app.UseBlazorFrameworkFiles();
app.UseStaticFiles();
#endif

app.MapControllers();

#if DEBUG

app.MapRazorPages();
app.MapFallbackToFile("index.html");

#endif

app.Run();
```