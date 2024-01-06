**DTOs** (Data Transfer Objects) are data strucutres used to encapsulate and trnasfer data (entities) between different application layers and components. They tailored to share only the necessary information for a specific operation.

You'd usually need two DTOs for any given entity, A [**Listing**](#listing-dto) DTO, and an [**Upsert**](#view-upsert-dto) DTO.

Optionally, if you want to obfuscate the ID, you can create a [custom attribute ](#hash-id-attribute) class and decorate the ID property in your DTOs with an attribute that's inheriting **`JsonHashIdConverterAttribute<T>`** attribute.

```hl_lines="10-13"
StockPlusPlus
|
├─ StockPlusPlus.API
├─ StockPlusPlus.Data
├─ StockPlusPlus.Functions
├─ StockPlusPlus.Shared
│   ├── ActionTrees
│   ├── Constants
│   ├── DTOs
│		├── ProductCategory
│			├── _ProductCategoryHashId.cs	<--- (Hash ID Attribute)
│			├── ProductCategoryDTO.cs		<--- (Upsert DTO)
│			├── ProductCategoryListDTO.cs	<--- (Listing DTO)
│   ├── Enums
│
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web
```

## Listing DTO
You usually don't need to display all the properties of an entity in a list. So you can create a DTO that only contains the properties you need to display.

A Listing DTO must inherit from **`ShiftEntityListDTO`** class.
```C# hl_lines="1"
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

## View & Upsert DTO

A view & upsert DTO is used for Insrting, Viewing, and Updating a single entity.
It must inherit from **`ShiftEntityViewAndUpsertDTO`** class.
```C# hl_lines="1"
public class ProductCategoryDTO : ShiftEntityViewAndUpsertDTO
{
    [_ProductCategoryHashId]
    public override string? ID { get; set; }

    [Required]
    public string Name { get; set; } = default!;

    public string? Description { get; set; }

    public string? Code { get; set; }

    [JsonConverter(typeof(JsonStringEnumConverter))]
    [Range(1, int.MaxValue, ErrorMessage = "Required")]
    public TrackingMethod TrackingMethod { get; set; }
}
```

## Hash ID Attribute

Notice how the ID field in the DTOs are decorated with the **`[_ProductCategoryHashId]`** attribute.
This can be used to obfuscate the ID of the entity. The attribute can be created by inheriting from **`JsonHashIdConverterAttribute<T>`**

```C# hl_lines="2"
public class _ProductCategoryHashId :
    JsonHashIdConverterAttribute<_ProductCategoryHashId>
{
    public _ProductCategoryHashId() : base(5) { }
}
```

!!! note
    **`base(5)`** is setting the minimum length of the Hash ID.
    This will change the IDs to random looking strings like (**`0W8y9`**, **`GVApJ`**) by using [**Hashids.net**](https://www.nuget.org/packages/Hashids.net) under the hood.


## Shift Entity Key And Name
There are certain features in the Framework that require you to specify a Key and Name property for entities and subsequently their DTOs.

This can be specified using the **``ShiftEntityKeyAndName``** attribute.

```C# hl_lines="1"
[ShiftEntityKeyAndName(nameof(ID), nameof(Name))]
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