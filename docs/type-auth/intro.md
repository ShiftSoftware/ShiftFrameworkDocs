# TypeAuth Intro

TypeAuth is a **Hierarchical** and **Strongly-Typed** Authorization System.   
   
In TypeAuth system, permissions are called **Action**s and they're defined in classes that are called [**Action Tree**](#action-tree)s

## Action Tree
An Action Tree is a collection of related Actions (Permissions).
=== "C#"

    ``` C#
    //Example of an ActionTree

    [ActionTree("Stock", "Actions Related to the Stock Module")]
    public class StockActions
    {
        public readonly static ReadWriteDeleteAction Brand = new("Brand");
        public readonly static ReadWriteDeleteAction ProductCategory = new("Product Category");
        public readonly static ReadWriteDeleteAction Product = new("Product");

        public readonly static ReadWriteDeleteAction Country = new("Country");

        [ActionTree("Data Level Access", "Data Level or Row-Level Access")]
        public class DataLevelAccess
        {
            public readonly static DynamicReadWriteDeleteAction Brand = new("Brand"); 
            public readonly static DynamicReadWriteDeleteAction ProductCategory = new("Product Category"); 
        }
    }
    ```

## Access Tree
An Access Tree is a JSON representation of the Action Trees that specify access on the Actions.   
A User (or any other sort of actor) may have one or more Access Trees.
``` JSON
{
    "StockActions": {
        "Brand": ['r'],
        "ProductCategory": ['r', 'w'],
        "Product": ['r', 'w', 'd'],
        "DataLevelAccess": {
            "Brand": ['r', 'w', 'd']
        }
    }
}
```
In the above example, whoever has this Access Tree has below access

 * **Read** Access on Brand
 * **Read/Write** Access on ProductCategory
 * **Read/Write/Delete** Access on Product
 * No Access on Country
 * **Read/Write/Delete** Access on DataLevelAccess.Brand
 * No Access on DataLevelAccess.ProductCategory


### Wild Card Access
Wild Card Access can be granted to entire Action Trees. the Access is applied to all existing, and future added Actions inside that Action Tree
``` JSON hl_lines="6" 
{
    "StockActions": {
        "Brand": ['r'],
        "ProductCategory": ['r', 'w'],
        "Product": ['r', 'w', 'd'],
        "DataLevelAccess": ['r', 'w', 'd']
    }
}
```
or
``` JSON hl_lines="2" 
{
    "StockActions": ['r', 'w']
}
```