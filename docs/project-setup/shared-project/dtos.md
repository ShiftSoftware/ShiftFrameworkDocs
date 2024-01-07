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
Sometimes it is required to hash an integer value into a random alphanumeric short string. This is mostly helpful when you need to obfesucate incremental IDs so they are not easy to guess.

Since hashing requires the same salt for encoding and dycoding, we need to keep track of the salt for each hashed ID. This is maintianed by adding a unique custom attribute to the ID property in every DTO it is referenced.

As the attributes will actually provide the salt to the hashing mechanism, and in order for IDs from different entities to have their own hasing, we use different attribute classes for each ID.

In the examples above ID field in the DTOs are decorated with the **`[_ProductCategoryHashId]`** attribute. This will obfuscate the ID of the entity.
The attribute can be created by inheriting from **`JsonHashIdConverterAttribute<T>`**

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


Make sure that if you need to obfesucate a specific ID, you have to apply the hash attribute to that ID in every DTO using it, whether as a primary or foriegn key.

## Shift Entity Key And Name
There are certain features in the Framework that requires you to specify a Key and Name property for entities and DTOs. In many places, especially in UI, we need to present the entities in a list (such as select or atuocomplete elements), and in which cases we only need to use two properties: one for the key (value), and the other for the display (text).

This can be specified using the **``ShiftEntityKeyAndName``** attribute. This attribute simply tells which property should be used as key and which should be used as a display name.

The attribute accepts exactly two parameters: the first parameter is the key (value) and the second is the name (text). Here is how you set it:

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

Once we define the entity's key and name through ShiftEntityKeyAndName attribute, we can use that entity later in UI elements without explicitly specifiying those fields:

```C#
    <ShiftAutocomplete Label="Brand"
                       @bind-Value="TheItem.Brand"
                       TEntitySet="BrandListDTO"
                       EntitySet="Brand"
                       For="() => TheItem.Brand" />
```