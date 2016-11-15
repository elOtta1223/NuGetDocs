# Create .NET Standard Packages with Visual Studio 2017

The [.NET Standard Library]((https://docs.microsoft.com/en-us/dotnet/articles/standard/library)) is a formal specification of .NET APIs intended to be available on all .NET runtimes, thus establishing greater uniformity in the .NET ecosystem. The .NET Standard Library defines a uniform set of BCL (Base Class Library) APIs for all .NET platforms to implement, independent of workload. It enables developers to produce PCLs that are usable across all .NET runtimes, and reduces if not eliminates platform-specific conditional compilation directives in shared code.

This guide will walk you through creating a nuget package targeting .NET Standard Library 1.4 with Visual Studio 2017 and NuGet 4.0. 

1. [Pre-requisites](#pre-requisites)
1. [Create the class library project](#create-the--net-core-class-library-project)
1. [Edit metadata in the .csproj file](#edit-metadata-in-the--csproj-file)
1. [Package the component](#package-the-component)
1. [Related topics](#related-topics)

## Pre-requisites

This walkthrough requires Visual Studio 2017 with the **.NET Core and Docker (Preview)** workload. You can install the Community edition for free from [visualstudio.com](https://www.visualstudio.com/), or use the Professional and Enterprise editions.

The require workload appears as follows in the Visual Studio installer:

![.NET Core and Docker (Preview) workload in the Visual Studio Installer](/images/BuildForNetStandard/NuGet4-01-Workload.png)

## Create the .NET Core class library project

1. In Visual Studio, **File > New > Project**, expand the **Visual C# > .NET Core** node, select **Class Library (Net Standard)**, change the name to AppLogger, and click OK.

    ![Create new class library project](/images/BuildForNetStandard/NuGet4-01-NewProject.png)

1. Change the build configuration to **Release**.
1. Right-click the `AppLogger (Portable)` in Solution Explorer, select **Properties**, select the **Build** tab, check the box for **XML documentation file**, and set the filename to just `AppLogger.xml`.

1. Add your code to the component, for example:

        namespace AppLogger
        {
            public class Logger
            {
                public void Log(string text)
                {
                    throw new NotImplementedException("Called Log");
                }
            }
        }

1. Build the project (with the Release configuration) and check that DLL and XML files are produced within the `bin\Release\netstadard1.4` folder.

## Edit metadata in the .csproj file

With NuGet 4.0 and .NET Core projects, package metadata is contained directly in the `.csproj` file instead of external files such as a `.nuspec'. A full description of that metadata is found in [Adding nuget pack as an MSBuild target](/ndocs/schema/csproj-pack-target).

1. Right-click the project in Solution Explorer and select **Edit AppLogger.csproj**.
1. Locate the `PropertyGroup` that contains a single `TargetFramework` element and motify it as follows. Note that `TargetFramework` becomes `TargetFrameworks` (plural):

    <PropertyGroup>
      <TargetFrameworks>netstandard1.4</TargetFrameworks>
        <PackageId>AppLogger.YOUR_NAME</PackageId>
        <PackageVersion>1.0.0</PackageVersion>
        <Authors>YOUR_NAME</Authors>
        <Description>Awesome application logging utility</Description>
        <PackageRequireLicenseAcceptance>false</PackageRequireLicenseAcceptance>
        <PackageReleaseNotes>First release</PackageReleaseNotes>
        <Copyright>Copyright 2016 (c) Contoso Corporation. All rights reserved.</Copyright>
        <PackageTags>logger logging logs</PackageTags>
    </PropertyGroup>

1. Right-click the solution and select **Build Solution** to again generate all the files for the package, this time with the correct metadata.


## Package the component

NuGet 4.0 supports a pack target using MSBuild when the project contains the necessary package metadata, as was added in the previous section. To invoke MSBuild in this way, simply specify the pack target on the command line:

    msbuild /t:pack /p:Configuration=Release

This will generate `AppLogger.YOUR_NAME.1.0.0.nupkg` in the `bin\Release` folder by default, as it builds that configuration. If you omit the `/p` switch, the default release will be `Debug`.

Opening this file in a tool like the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) and expanding all the nodes, you'll see the following contents:

![NuGet Package Explorer showing the AppLogger package](/images/BuildForNetStandard/NuGet4-03-PackageExplorer.png)

<div class="block-callout-info">
    <strong>Note</strong><br>
    A .nupkg file is just a ZIP file with a different extension. You can also examine package contents, then, by change .nupkg to .zip, but remember to restore the extension before uploading a package to nuget.org.
</div>

To make your package available to other developers,  follow the instructions on [Publish a package](/ndocs/create-packages/publish-a-package).

## Related topics

* [Nuspec Reference](/ndocs/schema/nuspec)
* [Symbol packages](/ndocs/create-packages/symbol-packages)
* [Dependency Versions](/ndocs/create-packages/dependency-versions)
* [Supporting Multiple .NET Framework Versions](/ndocs/create-packages/supporting-multiple-target-frameworks)
* [Include MSBuild props and targets in a package](/ndocs/create-packages/creating-a-package#including-msbuild-props-and-targets-in-a-package)
* [Creating Localized Packages](/ndocs/create-packages/creating-localized-packages)
* [.NET Standard Library documentation](https://docs.microsoft.com/en-us/dotnet/articles/standard/library)
* [Porting to .NET Core from .NET Framework](https://docs.microsoft.com/en-us/dotnet/articles/core/porting/index)