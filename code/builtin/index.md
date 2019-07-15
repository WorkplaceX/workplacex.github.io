---
title: BuiltIn
pageindex: 11
active: 1
--- 

# BuiltIn
The framework allows to "build in" database records into C# source code under certain circumstances. These C# "BuiltIn" records can be used in the deployment process to populate (merge) the target database with new data records. Or the "BuiltIn" records can be used at runtime time in the application.

## Code Example
Let's add a new sql script file to the cli. File name: "Application.Cli\SqlScript\City.sql"
```sql
CREATE TABLE Country
(
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(256) NOT NULL UNIQUE,
)

GO
CREATE TABLE City
(
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(256) NOT NULL UNIQUE,
    CountryId INT FOREIGN KEY REFERENCES Country(Id) NOT NULL,
)

GO
CREATE VIEW CountryBuiltIn AS
SELECT
    Id,
    Name AS IdName, -- Used for tables referencing table Country.Id
    Name
FROM
    Country

GO
CREATE VIEW CityBuiltIn AS
SELECT
    City.Id,
    City.Name,
    City.CountryId,
    (SELECT Country.Name FROM Country Country WHERE Country.Id = City.CountryId) AS CountryIdName
FROM
    City City
```

Now let's deploy the sql script to the database and run the following cli script:
```cmd
.\cli.cmd deployDb
```
The above cli script can be run multiple times. It registers in the sql table "FrameworkScript" the scripts which have been run on the current database.

Next we generate C# classes for City and Country like this:
```
.\cli.cmd generate
```
The new C# classes are found in the file: "Application.Database\Database\Database.cs"

Let's populate the sql tables with some data. Typically, this master data is added manually by an end user.

```sql
/* Country */
INSERT INTO Country (Name)
SELECT 'Germany'
UNION ALL
SELECT 'France'
UNION ALL
SELECT 'Italy'

/* City */
INSERT INTO City (Name, CountryId)
SELECT 'Berlin', (SELECT Id FROM Country WHERE Name = 'Germany')
UNION ALL
SELECT 'Frankfurt', (SELECT Id FROM Country WHERE Name = 'Germany')
UNION ALL
SELECT 'Paris', (SELECT Id FROM Country WHERE Name = 'France')
UNION ALL
SELECT 'Rom', (SELECT Id FROM Country WHERE Name = 'Italy')
```

Later we decide to incorporate the manually entered master data into the source code.

This is accomplished by overriding the method CommandGenerateBuiltIn:
```csharp
/// <summary>
/// Command line interface application.
/// </summary>
public class AppCliMain : AppCli
{
    protected override void CommandGenerateBuiltIn(List<GenerateBuiltInItem> list)
    {
        var countryList = Data.Select(Data.Query<CountryBuiltIn>());
        list.Add(GenerateBuiltInItem.Create(countryList));

        var cityList = Data.Select(Data.Query<CityBuiltIn>());
        list.Add(GenerateBuiltInItem.Create(cityList));
    }

    protected override void CommandDeployDbBuiltIn(List<DeployDbBuiltInItem> list)
    {
        list.Add(DeployDbBuiltInItem.Create(CountryBuiltInTableApplicationCli.RowList, nameof(CountryBuiltIn.Name), null));
        list.Add(DeployDbBuiltInItem.Create(CityBuiltInTableApplicationCli.RowList, nameof(CityBuiltIn.Name), null));
    }
}
```

Now lets run
```
.\cli.cmd generate
```
This generates the following C# code in file "Application.Cli\Database\DatabaseBuiltIn.cs"
```csharp
public static class CountryBuiltInTableApplicationCli
{
    public static List<CountryBuiltIn> RowList
    {
        get
        {
            var result = new List<CountryBuiltIn>();
            result.Add(new CountryBuiltIn() { Id = 1, IdName = "Germany", Name = "Germany" });
            result.Add(new CountryBuiltIn() { Id = 2, IdName = "France", Name = "France" });
            result.Add(new CountryBuiltIn() { Id = 3, IdName = "Italy", Name = "Italy" });
            return result;
        }
    }
}

public static class CityBuiltInTableApplicationCli
{
    public static List<CityBuiltIn> RowList
    {
        get
        {
            var result = new List<CityBuiltIn>();
            result.Add(new CityBuiltIn() { Id = 1 Name = "Berlin", CountryId = 1, CountryIdName = "Germany" });
            result.Add(new CityBuiltIn() { Id = 2, Name = "Frankfurt", CountryId = 1, CountryIdName = "Germany" });
            result.Add(new CityBuiltIn() { Id = 3, Name = "Paris", CountryId = 2, CountryIdName = "France" });
            result.Add(new CityBuiltIn() { Id = 4, Name = "Rom", CountryId = 3, CountryIdName = "Italy" });
            return result;
        }
    }
}
```
Cli deployDb command finally deploys and merges records.
```
.\cli.cmd deployDb
```

Further typical examples are:

## Configuration
The framework allows for example to configure column header text or column read-only. At a later stage such configuration data might be viewed as part of the software and get "BuiltIn" to the program.

## Language
The running application can be translated on the fly into new languages. The data is stored on the database. Later the new language should become part of the deployment process. Now the data needs to be copied into the C# "BuiltIn" zone.

## User, Role and Privilege
The application allows to add new users, roles and privileges to database tables. Later the user "Admin" might get referenced at compile time to evaluate certain access control. The same might be true of privileges.