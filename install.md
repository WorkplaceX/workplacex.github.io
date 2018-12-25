---
title: Install
path: install
pageindex: 05
active: 1
--- 

# Getting Started

Demo application is a comprehensive example to get started.

## Clone
```cmd
git clone https://github.com/WorkplaceX/ApplicationDemo.git --recursive
```
Recursive argument enables additional cloning of submodule (Framework).

## Command Line Interface (CLI)
In the root folder type cli for framework cli.
```cmd
cd Application
.\cli.cmd
```
This will show all available commands of framework CLI:

![Cli](https://raw.githubusercontent.com/WorkplaceX/Framework/master/Doc/Cli.png)

## ConnectionString
Set ConnectionString with cli:
```cmd
.\cli.cmd config ConnectionString="Data Source=localhost; Initial Catalog=Application; Integrated Security=True;"
```

## Deploy Sql Tables
Run deploy script from command line interface
```cmd
.\cli.cmd deployDb
```

## Start
Start and serve the application from command line
```cmd
.\cli.cmd start
```

Web browser opens on "https://localhost:56094/"
