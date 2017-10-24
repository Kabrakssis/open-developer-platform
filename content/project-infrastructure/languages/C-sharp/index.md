---
date: 2017-07-20T00:11:02+01:00
title: C#
type: index
weight: 120
---

C# is an object-oriented language that runs on the .NET Framework; feel free to browse the [Foundation hosted projects written in C#](https://github.com/symphonyoss?language=c%23); you can also browse the packages publicly available for consumption on [NuGet symphonyoss profile](https://nuget.org/profiles/symphonyoss).

Below are listed some of the most common processes that are involved in a C# project lifecycle and how they interact with the project infrastructure provided by the Foundation.

## Build
The easiest way to locally build a C# project is to [use Visual Studio](https://msdn.microsoft.com/en-us/library/5tdasz7h.aspx), although it's also possible to use the [MSBuild](https://msdn.microsoft.com/en-us/library/dd393574.aspx) command-line tool; you can find more info on [Microsoft Developer Network](https://msdn.microsoft.com/en-us/library/cyz1h6zd.aspx).

There is an open source, cross-platform alternative to the .NET Framework implementation called [Mono](http://www.mono-project.com/), based on the [ECMA standards](http://www.mono-project.com/docs/about-mono/languages/ecma/) for C# and the Common Language Runtime, however not all .NET applications can run on this platform, therefore it's safer to run all builds on a Windows environment with .NET Framework installed.

## Release on NuGet
NuGet is the package manager for the Microsoft development platform. The [NuGet client tools](https://docs.nuget.org/ndocs/guides/install-nuget) provide the ability to produce and consume packages. The NuGet Gallery is the central package repository used by all package authors and consumers.

Using the command-line, you can invoke the following command:
```
nuget pack project.csproj -IncludeReferencedProjects -Prop Configuration=Release
```

A badge can be added at the top of the project's root-level `README.md` file, using the following Markdown syntax:
```
[![NuGet Packages Status](https://img.shields.io/nuget/v/SymphonyOSS.RestApiClient.svg?maxAge=2592000)](https://www.nuget.org/packages/SymphonyOSS.RestApiClient/)
```
This will appear as follows and link to the MyGet project page.
[![NuGet Packages Status](https://img.shields.io/nuget/v/SymphonyOSS.RestApiClient.svg?maxAge=2592000)](https://www.nuget.org/packages/SymphonyOSS.RestApiClient/)

You can also run this process continuously on each commit and publish pre-releases by [integrating with MyGet](https://symphonyoss.atlassian.net/wiki/spaces/FM/pages/73564181/Continuous+Integration#ContinuousIntegration-MyGet).