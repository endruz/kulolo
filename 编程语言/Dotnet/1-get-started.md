# Get Started

- [Get Started](#get-started)
  - [Install](#install)
  - [Check the installed .Net](#check-the-installed-net)
    - [Check SDK version](#check-sdk-version)
    - [Check runtime version](#check-runtime-version)
    - [Detailed information](#detailed-information)
    - [Specify .NET SDK version](#specify-net-sdk-version)
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

```powershell
> dotnet --list-sdks
5.0.403 [C:\Program Files\dotnet\sdk]
6.0.310 [C:\Program Files\dotnet\sdk]
7.0.202 [C:\Program Files\dotnet\sdk]
```

### Check runtime version

```powershell
> dotnet --list-runtimes
Microsoft.AspNetCore.App 3.1.21 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 3.1.32 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 5.0.12 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 5.0.17 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 6.0.15 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 7.0.4 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.NETCore.App 3.1.21 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 3.1.32 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 5.0.12 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 5.0.17 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 6.0.15 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 7.0.4 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.WindowsDesktop.App 3.1.21 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 3.1.32 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 5.0.12 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 5.0.17 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 6.0.15 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 7.0.4 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
```

### Detailed information

```powershell
> dotnet --info
.NET SDK:
 Version:   7.0.202
 Commit:    6c74320bc3

Runtime Environment:
 OS Name:     Windows
 OS Version:  10.0.22621
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\7.0.202\

Host:
  Version:      7.0.4
  Architecture: x64
  Commit:       0a396acafe

.NET SDKs installed:
  5.0.403 [C:\Program Files\dotnet\sdk]
  6.0.310 [C:\Program Files\dotnet\sdk]
  7.0.202 [C:\Program Files\dotnet\sdk]

.NET runtimes installed:
  Microsoft.AspNetCore.App 3.1.21 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 3.1.32 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 5.0.12 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 5.0.17 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 6.0.15 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 7.0.4 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 3.1.21 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 3.1.32 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 5.0.12 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 5.0.17 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 6.0.15 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.NETCore.App 7.0.4 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.WindowsDesktop.App 3.1.21 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
  Microsoft.WindowsDesktop.App 3.1.32 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
  Microsoft.WindowsDesktop.App 5.0.12 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
  Microsoft.WindowsDesktop.App 5.0.17 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
  Microsoft.WindowsDesktop.App 6.0.15 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]
  Microsoft.WindowsDesktop.App 7.0.4 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]

Other architectures found:
  x86   [C:\Program Files (x86)\dotnet]
    registered at [HKLM\SOFTWARE\dotnet\Setup\InstalledVersions\x86\InstallLocation]

Environment variables:
  Not set

global.json file:
  Not found

Learn more:
  https://aka.ms/dotnet/info

Download .NET:
  https://aka.ms/dotnet/download
```

### Specify .NET SDK version

Check the version of the .NET SDK used by dotnet commands.

```powershell
> dotnet --version
7.0.202
```

We can use `global.json` to specify .NET SDK version. If `global.json` is not available, the latest version is used by default.

The .NET SDK looks for a `global.json` file in the current working directory (which isn't necessarily the same as the project directory) or one of its parent directories.

For example, the following global.json file selects 6.0.300 or any later feature band or patch for 6.0 that is installed on the machine:

```json
{
  "sdk": {
    "version": "6.0.300",
    "rollForward": "latestFeature"
  }
}
```

Some detailed configuration can be seen in [here](https://learn.microsoft.com/en-us/dotnet/core/tools/global-json).

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
