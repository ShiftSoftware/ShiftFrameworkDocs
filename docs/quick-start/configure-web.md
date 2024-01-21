##Configur Program.cs
1. Here's an example of Program.cs

```C#
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.Extensions.Localization;
using ShiftSoftware.ShiftBlazor.Extensions;
using ShiftSoftware.ShiftBlazor.Services;
using ShiftSoftware.ShiftIdentity.Blazor.Extensions;
using ShiftSoftware.ShiftIdentity.Blazor.Handlers;
using ShiftSoftware.ShiftIdentity.Dashboard.Blazor.Extensions;
using ShiftSoftware.TypeAuth.Blazor.Extensions;
using StockPlusPlus.Shared.ActionTrees;
using StockPlusPlus.Web;
using System.Globalization;

[assembly: RootNamespace("StockPlusPlus.Web")]
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");


builder.Services.AddScoped(sp =>
{
    var httpClient = new HttpClient(sp.GetRequiredService<TokenMessageHandlerWithAutoRefresh>())
    {
        BaseAddress = new Uri(builder.Configuration!.GetValue<string>("BaseURL")!)
    };

    return httpClient;
});

var baseUrl = builder.Configuration!.GetValue<string>("BaseURL")!;

builder.Services.AddShiftBlazor(config =>
{
    config.ShiftConfiguration = options =>
    {
        options.BaseAddress = baseUrl!;
        options.ApiPath = "/api";
        options.ODataPath = "/odata";
        options.UserListEndpoint = baseUrl.AddUrlPath("odata/IdentityPublicUser");
        options.AdditionalAssemblies = new[] { typeof(ShiftSoftware.ShiftIdentity.Dashboard.Blazor.ShiftIdentityDashboarBlazorMaker).Assembly };
        options.AddLanguage("en-US", "English")
               .AddLanguage("ar-IQ", "Arabic", true)
               .AddLanguage("en-US", "English RTL", true)
               .AddLanguage("ku-IQ", "Kurdish", true);
    };
    //config.SyncfusionLicense = builder.Configuration.GetValue<string>("SyncfusionLicense");
});

builder.Services.AddShiftIdentity("stock-plus-plus-dev", baseUrl, baseUrl);

builder.Services.AddShiftIdentityDashboardBlazor(x =>
{
    x.ShiftIdentityHostingType = ShiftSoftware.ShiftIdentity.Core.ShiftIdentityHostingTypes.Internal;
    x.LogoPath = "/img/shift-full.png";
    x.Title = "Stock Plus Plus";
    x.DynamicTypeAuthActionExpander = async () =>
    {
        //var httpService = builder.Services.BuildServiceProvider().GetRequiredService<HttpService>();

        //var projects = await httpService.GetAsync<ODataDTO<ProjectListDTO>>("/odata/project");

        //ToDoActions.DataLevelAccess.Projects.Expand(projects.Data!.Value.Select(x => new KeyValuePair<string, string>(x.ID!, x.Name!)).ToList());

        //ToDoActions.DataLevelAccess.Statuses.Expand(Enum.GetValues<ToDo.Shared.Enums.ToDoStatus>().Select(x => new KeyValuePair<string, string>(((int)x).ToString(), x.Describe())).ToList());
    };
    x.AddCompanyBranchCustomField("Username", "User Name")
        .AddCompanyBranchCustomField("Password", true)
        .AddCompanyCustomField("AutolineLink","Autoline Link");
});

builder.Services.AddTypeAuth(x =>
    x
    .AddActionTree<ShiftSoftware.ShiftIdentity.Core.ShiftIdentityActions>()
    .AddActionTree<StockActionTrees>()
    .AddActionTree<SystemActionTrees>()
);

var host = builder.Build();

var setMan = host.Services.GetRequiredService<SettingManager>();

var culture = setMan.GetCulture();

CultureInfo.DefaultThreadCurrentCulture = culture;
CultureInfo.DefaultThreadCurrentUICulture = culture;

await host.RefreshTokenAsync(50);

await host.RunAsync();
```
