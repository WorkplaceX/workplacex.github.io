---
title: Install
path: install
pageindex: 05
active: 1
--- 

# Getting Started

"Demo" application is a comprehensive example to get started with. The following two components already need to be installed as prerequisite:

* [Node.js](https://nodejs.org/en/) (LTS Version)
* [.NET Core](https://dotnet.microsoft.com/download) (Version 2.1)

## Git Clone
```cmd
git clone https://github.com/WorkplaceX/ApplicationDemo.git --recursive
```
Argument "--Recursive" clones also necessary submodule (Framework).

## Command Line Interface (CLI)
The framework provides a command line interface (CLI) with all necessary functions like build, deploy and so on. In the root folder type cli.
```cmd
cd ApplicationDemo
.\cli.cmd
```
All framework CLI commands are shown:

![Cli](Doc/Cli.png)

## ConnectionString
Set ConnectionString with command line interface:
```cmd
.\cli.cmd config ConnectionString="Data Source=localhost; Initial Catalog=Application; Integrated Security=True;"
```

It creates or updates the files: "ConfigCli.json" and "ConfigWebServer.json".

{:.note}
**Note:** Why does "ConfigCli.json" and "ConfigWebServer.json" have two ConnectionStrings? The framework needs a couple of sql tables (prefixed with "Framework") to run. These tables can be deployed to a second empty database. This makes it possible to wiretap an existing production database initially with a ready only user login. Like this it's possible to test and demonstrate the frameworks capabilities to customers in a non-critical way.

## Deploy Sql Tables
Run sql deploy script from command line interface to deploy necessary sql tables and views to database.
```cmd
.\cli.cmd deployDb
```

{:.note}
**Note:** You can run the command "deployDb" multiple times. The database table "FrameworkScript" keeps track of the executed scripts. No script is executed twice on the same database. The sql scripts being executed are in folder: "Application.Cli\SqlScript\"

## Build
Build the application from command line:
```cmd
.\cli.cmd build
```

## Start
Serve the application from command line:
```cmd
.\cli.cmd start
```

Web browser opens on "http://localhost:56098/" and serves "Demo" application.

## Custom Sql Table and View

Finally let's wiretap any other sql table or view.

Following cli command generates for every table and view a Csharp code class in the file "Application\Application.Database\Database.cs"

```cmd
.\cli.cmd generate
```

Follow this article on how to display an sql table or view in the web application: **[Code Examples](code)**
