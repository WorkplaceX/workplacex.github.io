---
title: Code
path: code
pageindex: 10
active: 1
--- 

# Code Examples

Following simple code examples help to get started with the framework.

## Hello World

Render simple "Hello World" text to the web application. The class AppMain represents the main application and derives from frameworks AppJson class. The parameter less constructor is used for json deserialization. The second constructor is used when the object is created programmatically. For example with the extension method ComponentCreate();

```csharp
public class AppMain : AppJson
{
    public AppMain() : this(null) { }

    public AppMain(ComponentJson owner) : base(owner) { }

    protected override Task InitAsync()
    {
        this.ComponentCreate<Html>().TextHtml = "Hello World!";
        return base.InitAsync();
    }
}
```

## Data Grid
Now let's add a data grid to our web application. The extension method ComponentCreate<Grid>(); creates a reference point in our json component tree. The method LoadAsync(); initiates the loading of data. In the method GridQuery(); we define the linq query to be executed (either to database or in memory). Additionally in this example we do a primary filtering on "IsActive" records.

```csharp
public class AppMain : AppJson
{
    public AppMain() : this(null) { }

    public AppMain(ComponentJson owner) : base(owner) { }

    protected override async Task InitAsync()
    {
        await this.ComponentCreate<Grid>().LoadAsync(); // Create a reference point in component tree and initiate loading.
    }

    protected override IQueryable GridQuery(Grid grid)
    {
        return UtilDal.Query<HelloWorld>().Where(item => item.IsActive == true); // Define the linq query.
    }
}
```
