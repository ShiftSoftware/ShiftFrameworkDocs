The ``ShiftSoftware.ShiftFrameworkTestingTools`` package includes attributes to decorate test calsses and test methods to control the serial execution of tests. 

## TestCaseOrderer
Should be placed on test classes.

```C# hl_lines="5"
using ShiftSoftware.ShiftFrameworkTestingTools;

namespace StockPlusPlus.Test.Tests;

[TestCaseOrderer(Constants.OrdererTypeName, Constants.OrdererAssemblyName)]
public class Test
{
}
```
## TestPriority
Should be placed on Test Methods

```C# hl_lines="5 11 20"
using ShiftSoftware.ShiftFrameworkTestingTools;

namespace StockPlusPlus.Test.Tests;

[TestCaseOrderer(Constants.OrdererTypeName, Constants.OrdererAssemblyName)]
public class TestPriority
{
    private static int COUNT = 0;

    [Fact]
    [TestPriority(2)]
    public void One()
    {
        COUNT++;

        Assert.Equal(2, COUNT);
    }

    [Fact]
    [TestPriority(1)]
    public async Task Two()
    {
        COUNT++;

        await Task.Delay(2000);

        Assert.Equal(1, COUNT);
    }
}

```
