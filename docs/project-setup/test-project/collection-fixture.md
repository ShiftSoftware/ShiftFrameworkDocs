We can create a Collection Definition for the Custom Web Application Factory to decorate the test classes.

```C#
[CollectionDefinition("API Collection")]
public class APICollection : ICollectionFixture<CustomWebApplicationFactory>
{
}
```

You can then use that to decorate your test classes.

```C# hl_lines="1"
[Collection("API Collection")]
public class Test
{
    [Fact]
    public void TestOne()
    {
    }
}
```