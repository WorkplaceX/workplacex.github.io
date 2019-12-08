---
title: Install on Local IIS Server
--- 

# Local IIS Server
With the following prerequisites WorkplaceX runs on a local IIS Server on Windows 10

In order to make .NET Core applications run on local IIS Server install "dotnet-hosting-win.exe"

* [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) (dotnet-hosting-3.1.0-win.exe)

For Angular server side rendering to run on IIS Server "iisnode-full.msi" has to be installed

* [https://github.com/Azure/iisnode/releases](https://github.com/Azure/iisnode/releases) (iisnode-full-v0.2.26-x64.msi)

After successful installation Windows "add remove programs" should look like this:

![IIS Node](Doc/InstallIISNode.png)

