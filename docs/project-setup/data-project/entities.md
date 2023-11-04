Shift Framework is using [EF Core 6](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/6.0.20). And most entities need to inherit from **``ShiftEntity``**

```hl_lines="6 7"
StockPlusPlus
|
├─ StockPlusPlus.API
├─ StockPlusPlus.Data
│   ├── AutoMapperProfiles
│   ├── Entities
│       ├── Brand.cs
│   ├── Migrations
│   ├── ReplicationModels
│   ├── Repositories
│   ├── DB.cs
│ 
├─ StockPlusPlus.Functions
├─ StockPlusPlus.Shared
├─ StockPlusPlus.Test
├─ StockPlusPlus.Web
```

## Shift Entity

Entities inheriting from **``ShiftEntity``** seamlessly integrate with the framework. 

```C#
public class Brand : ShiftEntity<Brand>
{
    public string Name { get; set; } = default!;
    public string? Description { get; set; }
    public string? Code { get; set; }
}
```

The **``ShiftEntity``** base class will add the following properties:

| Property                 | Type            | Description                                                                              |
| ------------------------ | --------------  | ---------------------------------------------------------------------------------------- |
| **ID**                   | long            | The primary key of the entity.                                                           |
| **CreateDate**           | DateTimeOffset  | The UTC datetime assigned during creation and never updated.                             |
| **LastSaveDate**         | DateTimeOffset  | The UTC datetime of the last time this entity is saved (Created, Updated, or Deleted)).  |
| **LastReplicationDate**  | DateTimeOffset? | The UTC datetime of the last time the entity has been replicated to CosmosDB.            |
| **CreatedByUserID**      | long?           | The User ID assigned during creation and never updated.                                  |
| **LastSavedByUserID**    | long?           | The User ID of the last user that has saved the entity (Created, Updated, or Deleted).   |
| **IsDeleted**            | bool            | Soft Delete Flag.                                                                        |
| **RegionID**             | long?           | The Region ID assigned during creation and never updated.                                |
| **CompanyID**            | long?           | The Company ID assigned during creation and never updated.                               |
| **CompanyBranchID**      | long?           | The Company Branch ID assigned during creation and never updated.                        |


## Temporal (System Versioned)

Use the **``TemporalShiftEntity``** attribute to enable Temporal Table (Currently available on SQL Server Only).

```C# hl_lines="1"
[TemporalShiftEntity]
public class Brand : ShiftEntity<Brand>
{
    public string Name { get; set; } = default!;
    public string? Description { get; set; }
    public string? Code { get; set; }
}
```


## Shift Entity Key And Name
There are certain features in the Framework that requires you to specify a Key and Name property for an entity.  

This can be specified using the **``ShiftEntityKeyAndName``** attribute.

```C# hl_lines="2"
[TemporalShiftEntity]
[ShiftEntityKeyAndName(nameof(ID), nameof(Name))]
public class Brand : ShiftEntity<Brand>
{
    public string Name { get; set; } = default!;
    public string? Description { get; set; }
    public string? Code { get; set; }
}
```
