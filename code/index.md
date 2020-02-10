---
title: Simple Source Code Examples in C# Sharp
--- 

# Code Examples

Get started with the following simple code examples:

* [Hello World Text](#hello-world-text) (Show simple hello world text)
* [Data Grid](#data-grid) (Add sql server data grid to web application)
* [Data Annotation](#data-annotation) (Green arrow up for positive numbers)
* [Data Lookup Window](#data-lookup-window) (Data lookup up or autocomplete window)
* [Customize Design](#customize-design) (Change web site template)

## Video Tutorial
This short 2:12 minutes video shows how to add a hello world text, a data grid and a lookup window. It follows the code examples described on this page.

<div class="youtube-container">
<iframe width="560" height="315" src="https://www.youtube.com/embed/TnYavCQ2pgM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## Hello World Text
Let's render a simple "Hello World" text to the web application. The class AppMain represents the main application and derives from frameworks AppJson class. The parameterless constructor is used for json deserialization. The second constructor is used when the object is created programmatically. For example with the extension method ComponentCreate();

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
Now let's add a data grid to our web application. The extension method ComponentCreate<Grid>(); creates a reference point in our json component tree. The method LoadAsync(); initiates the loading of data. In the method GridQuery(); we define the linq query to be executed (either to database or in memory). Additionally, in this example we do a primary filtering on "IsActive" records and sort the grid by default by the column "Text".

```csharp
public class AppMain : AppJson
{
    public AppMain() : this(null) { }

    public AppMain(ComponentJson owner) : base(owner) { }

    protected override async Task InitAsync()
    {
        // Create a reference point in component tree and initiate loading.
        await this.ComponentCreate<Grid>().LoadAsync(); 
    }

    protected override IQueryable GridQuery(Grid grid)
    {
        // Define linq query with default sorting.
        return Data.Query<HelloWorld>().Where(item => item.IsActive == true).OrderBy(item => item.Text);
    }
}
```

Following screenshot shows on the left-hand side Management Studio with the data table and on the right-hand side the same table in the web application.

By default it comes with

* Paging (Vertically and horizontally)
* Sorting
* Filtering
* Adding new records

{% include image.html url="Doc/Grid.png" text="Data grid" %}

## Data Annotation
Adding visual icons to your data application brings a lot of benefits. For example just adding the phone icon [Font Awesome phone](https://fontawesome.com/icons/phone?style=solid) to the telephone column makes it look much more appealing to the end user.

Now let's consider another example. If for example the number is positive a green arrow up should be shown and if the number is negative a red arrow down should be shown. For this we override the method GridCellAnnotation(); like this:

```csharp
protected override void GridCellAnnotation(Grid grid, string fieldName, GridRowEnum gridRowEnum, Row row, GridCellAnnotationResult result)
{
    HelloWorld helloWorld = row as HelloWorld;
    if (fieldName == nameof(HelloWorld.Number))
    {
        if (helloWorld?.Number > 0)
        {
            result.HtmlLeft = "<i class='fas fa-arrow-up green'></i>";
        }
        if (helloWorld?.Number < 0)
        {
            result.HtmlLeft = "<i class='fas fa-arrow-down red'></i>";
        }
        if (helloWorld?.Number == 0)
        {
            result.HtmlLeft = "<i class='fas fa-arrow-right'></i>";
        }
    }
}
```

Now the data grid looks like this:

{% include image.html url="Doc/GridAnnotation.png" text="Data grid with annotation" %}

# Data Lookup Window

Auto complete or data lookup windows allow users to select items from dynamic lists. Following code example shows the column "Text" with a country auto complete window.

Override the method GridLookupQuery(); to return the query for the look up window. And finally override the method GridLookupSelected(); to return the text that should go into the edit cell.

```csharp
public class AppMain : AppJson
{
	public AppMain() : this(null) { }

	public AppMain(ComponentJson owner) : base(owner) { }

	protected override async Task InitAsync()
	{
		await this.ComponentCreate<Grid>().LoadAsync();
	}

	protected override IQueryable GridQuery(Grid grid)
	{
		return Data.Query<HelloWorld>().OrderBy(item => item.Text);
	}

	protected override IQueryable GridLookupQuery(Grid grid, Row row, string fieldName, string text)
	{
		if (fieldName == nameof(HelloWorld.Text))
		{
			// Return country query for look up window on column "Text".
			return Data.Query<CountryDisplay>().Where(item => item.Country.StartsWith(text)).OrderBy(item => item.Country);
		}
		return null; // No lookup for other data columns
	}

        protected override string GridLookupRowSelected(Grid grid, string fieldName, GridRowEnum gridRowEnum, Row rowLookupSelected)
	{
		// User clicked a country row in the lookup window. Return the text that should go into the edit cell.
		return ((CountryDisplay)rowLookupSelected).Country;
	}
}
```

Following screen shot shows open lookup or auto complete window on column "Text" to select a country.

{% include image.html url="Doc/Lookup.png" text="Data grid with lookup window" %}

# Customize Design

The web site template is found at folder "Application\Website\". To develop and change the design type 

```cmd
npm start
```

An empty web site with styles, Bootstrap and Font Awesome references is shown at: http://localhost:8081/. The framework replaces the html tag "data-app" with the application. In order to change the color of the arrows in the example above update the style.css file like this:

```
.red {
    color: red;
}

.green {
    color: green;
}
```

After changing the template, it is necessary to run the command line build command ("-c" parameter stands for build web client only)

```cmd
.\cli.cmd build -c
```

