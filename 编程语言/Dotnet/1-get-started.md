# Get Started

- [Get Started](#get-started)
  - [Install](#install)
  - [Check the installed .Net](#check-the-installed-net)
    - [Check SDK version](#check-sdk-version)
    - [Check runtime version](#check-runtime-version)
    - [Detailed information](#detailed-information)
  - [What is .Net](#what-is-net)
    - [Binary distributions](#binary-distributions)
    - [Programming Languages](#programming-languages)
    - [Package Manager](#package-manager)
    - [.Net implementations](#net-implementations)
  - [Using .NET](#using-net)


## Install

Reference the [document](https://learn.microsoft.com/en-us/dotnet/core/install/) of microsoft.

## Check the installed .Net

### Check SDK version

```bash
$ dotnet --list-sdks
7.0.200 [/usr/share/dotnet/sdk]
```

### Check runtime version

```bash
$ dotnet --list-runtimes
Microsoft.AspNetCore.App 7.0.3 [/usr/share/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.NETCore.App 7.0.3 [/usr/share/dotnet/shared/Microsoft.NETCore.App]
```

### Detailed information

```bash
$ dotnet --info
.NET SDK:
 Version:   7.0.200
 Commit:    534117727b

Runtime Environment:
 OS Name:     ubuntu
 OS Version:  20.04
 OS Platform: Linux
 RID:         ubuntu.20.04-x64
 Base Path:   /usr/share/dotnet/sdk/7.0.200/

Host:
  Version:      7.0.3
  Architecture: x64
  Commit:       0a2bda10e8

.NET SDKs installed:
  7.0.200 [/usr/share/dotnet/sdk]

.NET runtimes installed:
  Microsoft.AspNetCore.App 7.0.3 [/usr/share/dotnet/shared/Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 7.0.3 [/usr/share/dotnet/shared/Microsoft.NETCore.App]

Other architectures found:
  None

Environment variables:
  Not set

global.json file:
  Not found

Learn more:
  https://aka.ms/dotnet/info

Download .NET:
  https://aka.ms/dotnet/download
```

## What is .Net

.NET is a free, cross-platform, open source developer platform for building many kinds of applications.

.NET is built on a high-performance runtime that is used in production by many high-scale apps.

### Binary distributions

- .NET SDK -- Set of tools, libraries, and runtimes for development, building, and testing apps.
- .NET Runtimes -- Set of runtimes and libraries, for running apps.

### Programming Languages

- C#
- F#
- Visual Basic

### Package Manager

NuGet is the package manager for .NET. It enables developers to share compiled binaries with each other.

[NuGet.org](https://www.nuget.org/) offers many popular packages from the community.

### .Net implementations

A .NET app is developed for one or more implementations of .NET.

There are four .NET implementations that Microsoft supports:

- .NET 5 (and .NET Core) and later versions
- .NET Framework
- Mono
- UWP

## Using .NET

.NET apps and libraries are built from source code and a project file, using the .NET CLI or an Integrated Development Environment (IDE) like Visual Studio.

Next, create a new application using the .NET 7.

```bash
$ mkdir hello-world
$ cd hello-world
# Create a new console application
$ dotnet new console
The template "Console App" was created successfully.

Processing post-creation actions...
Restoring /home/endruz/code/endruz/hello-world/hello-world.csproj:
  Determining projects to restore...
  Restored /home/endruz/code/endruz/hello-world/hello-world.csproj (in 62 ms).
Restore succeeded.
$ tree -L 1
.
├── Program.cs
├── hello-world.csproj
└── obj

1 directory, 2 files
```

Project file `hello-world.csproj`:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net7.0</TargetFramework>
    <RootNamespace>hello_world</RootNamespace>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

Source code `Program.cs`:

```C#
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

Restore the needed packages with `dotnet restore`.

```bash
$ dotnet restore
  Determining projects to restore...
  All projects are up-to-date for restore.
```

The app can be built and run with the .NET CLI:

```Bash
$ dotnet run
Hello, World!
```

It can also be built and run as two separate steps:

```Bash
$ dotnet build
MSBuild version 17.5.0-preview-23061-01+040e2a90e for .NET
  Determining projects to restore...
  All projects are up-to-date for restore.
  hello-world -> /home/endruz/code/endruz/hello-world/bin/Debug/net7.0/hello-world.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:00.72
$ ./bin/Debug/net7.0/hello-world
Hello, World!
```
