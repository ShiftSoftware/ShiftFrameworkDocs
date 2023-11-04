Enums are placed directly under Shared/Enums folder or they're nested if needed.

```hl_lines="10-12"
StockPlusPlus
|
├─ StockPlusPlus.API
├─ StockPlusPlus.Data
├─ StockPlusPlus.Functions
├─ StockPlusPlus.Shared
│   ├── ActionTrees
│   ├── Constants
│   ├── DTOs
│   ├── Enums
│           ├── TrackingMethod.cs
│ 
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web
```

<br />

Enums are simple by nature. There are only the below two special treatments that's usually needed when dealing with them in Shift Framework:

## 1.Description Attribute
We usually put a **[Description]** attribute that's available in **System.ComponentModel** on the enum values to give them a friendly name.  
```C# hl_lines="7 10 13 16 19"
using System.ComponentModel;

namespace StockPlusPlus.Shared.Enums.Product;

public enum TrackingMethod
{
    [Description("Serial")]
    Serial = 1,

    [Description("Batch/LOT")]
    Batch_LOT = 2,

    [Description("Weight/Volume")]
    Weight_Volume = 3,

    [Description("Kitting/Assembly")]
    Kitting_Assembly = 4,

    [Description("No Tracking")]
    NoTracking = 5,
}
```

!!! note
    The `Describe` extension method can be used on enum values to read the description string.
    `TrackingMethod.NoTracking.Describe()`

## JsonConverter Attribute
When Enums are used in DTOs, They need to be decorated by **[JsonConverter(typeof(JsonStringEnumConverter))]** attribute.

```C# hl_lines="12"
public class ProductCategoryListDTO : ShiftEntityListDTO
{
    [_ProductCategoryHashId]
    public override string? ID { get; set; }

    public string Name { get; set; } = default!;

    public string? Description { get; set; }

    public string? Code { get; set; }

    [JsonConverter(typeof(JsonStringEnumConverter))]
    public TrackingMethod? TrackingMethod { get; set; }
}
```