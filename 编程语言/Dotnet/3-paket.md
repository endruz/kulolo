# Paket

- [Paket](#paket)
  - [Install](#install)
  - [Initialize](#initialize)
  - [Related files](#related-files)
  - [Commands](#commands)

## Install

```powershell
# Create a manifest file(.config/dotnet-tools.json) in the root of current directory
dotnet new tool-manifest
# Install and add paket to the manifest file
dotnet tool install paket
```

`.config/dotnet-tools.json` must be checked into source control.

When cloning your code base, you can use `dotnet tool restore` to install all the tools that were added to the manifest file.

## Initialize

```powershell
# Initialize Paket by creating file paket.dependencies and directory paket-files
dotnet paket init
```

If you have a build script, e.g. `build.sh/build.cmd`, also make sure you add the last two commands before you execute your build:

```powershell
dotnet tool restore
dotnet paket restore
```

This will ensure Paket works in any .NET Core build environment.

Make sure to add the following entry to your .gitignore:

```sh
#Paket dependency manager
paket-files/
```

## Related files

Paket manages your dependencies with three core file types:

- `paket.dependencies`, where you specify your dependencies and their versions for your entire codebase.
- `paket.references`, a file that specifies a subset of your dependencies for every project in a solution.
- `paket.lock`, a lock file that Paket generates when it runs. When you check it into source control, you get reproducible builds.

You edit the `paket.dependencies` and `paket.references` files by hand as needed. When you run a paket command, it will generate the `paket.lock` file.

All three file types must be committed to source control.

## Commands

- `paket install` - Compute dependency graph, download dependencies and update projects.
- `paket outdated` - Find dependencies that have newer versions available.
- `paket update` - Update dependencies to their latest version..
- `paket restore` - Download the computed dependency graph.
- `paket remove` - Remove a dependency.
- ...
