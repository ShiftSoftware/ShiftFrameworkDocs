Actions and Action Trees are part of TypeAuth library.

**ActionTree** is a hierarchical representation of the actions that an actor is authorized to perform.  
It's a collection of actions that are organized into a tree structure.   

```C# hl_lines="1-2"
[ActionTree("Stock", "Stock")]
public class StockActionTrees
{
    public readonly static ReadWriteDeleteAction Brand = new("Brand");
    public readonly static ReadWriteDeleteAction ProductCategory = new("Product Category");
    public readonly static ReadWriteDeleteAction Product = new("Product");
}
```

Each node in the tree represents an action, and the child nodes represent sub-actions that are related to the parent action.

```C# hl_lines="8-13"
[ActionTree("Stock", "Stock")]
public class StockActionTrees
{
    public readonly static ReadWriteDeleteAction Brand = new("Brand");
    public readonly static ReadWriteDeleteAction ProductCategory = new("Product Category");
    public readonly static ReadWriteDeleteAction Product = new("Product");

    [ActionTree("Data Level Access", "Data Level or Row-Level Access")]
    public class DataLevelAccess
    {
        public readonly static DynamicReadWriteDeleteAction Brand = new("Brand"); 
        public readonly static DynamicReadWriteDeleteAction ProductCategory = new("Product Category"); 
    }
}
```

!!! note
    The **ActionTrees** and the **Actions** are simple strongly typed representations of the capabilities of the system that need to be authorized.  

    They're later used by the TypeAuth libraries to secure end-points, UI elements, and other resources.
