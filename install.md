---
title: Install
path: install
pageindex: 05
active: 1
--- 

# Getting Started

"Demo" application is a comprehensive example to get started with. The following two components need to be installed as prerequisite:

* .NET Core https://dotnet.microsoft.com/download (Version 2.1)
* nodejs https://nodejs.org/en/ (LTS Version)

## Git Clone
```cmd
git clone https://github.com/WorkplaceX/ApplicationDemo.git --recursive
```
Argument "--Recursive" enables additional cloning of submodule (Framework).

## Command Line Interface (CLI)
In the root folder type cli for framework cli.
```cmd
cd ApplicationDemp
.\cli.cmd
```
This will show all available framework CLI commands:

![Cli](Doc/Cli.png)

## ConnectionString
Set ConnectionString with command line interface:
```cmd
.\cli.cmd config ConnectionString="Data Source=localhost; Initial Catalog=Application; Integrated Security=True;"
```

## Deploy Sql Tables
Run sql deploy script from command line interface to deploy necessary sql tables and views.
```cmd
.\cli.cmd deployDb
```

## Start
Serve the application from command line:
```cmd
.\cli.cmd start
```

Web browser opens on "https://localhost:56094/" and serves "Demo" application.

## Custom Sql Table and View

Finally let's wiretap any other sql table or view.

Following cli command generates for every table and view a Csharp code class in the file "Application\Application.Database\Database.cs"

```cmd
.\cli.cmd generate
```

Follow the [code examples](code) article to display a table or view in the web application.
