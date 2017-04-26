Contributing to this project
============================

## Prerequisites

This project is actively developed using the following software.
It is highly recommended that anyone contributing to this library use the same
software.

1. [Visual Studio 2017][VS]
2. [Node.js][NodeJs]

### Optional additional software

Some projects in the Visual Studio solution require optional or 3rd party components to open.
They are not required to build the full solution from the command line using MSBuild,
but installing this software will facilitate an enhanced developer experience in Visual Studio.

1. [Node.js Tools for Visual Studio][NodeJsTools]

All other dependencies are acquired via NuGet or NPM.

## Building

To build this repository from the command line, you must first execute our init.ps1 script,
which downloads NuGet.exe and uses it to restore packages.
Assuming your working directory is the root directory of this git repo,
and you are running Windows PowerShell, the command is:

    .\init.ps1

Most of the repo may be built via building the solution file from Visual Studio 2017,
but for a complete build, build from the VS2017 Developer Command Prompt:

    .\build.ps1

This repo is structured such that it builds the NuGet package first, using MSBuild.
It then builds an NPM package that includes some of the outputs of MSBuild, along with
some javascript, for our NPM consumers who want a reasonable versioning story for their
NPM packages too.

## Testing

The Visual Studio 2017 Test Explorer will list and execute all tests.

## Pull requests

Pull requests are welcome! They may contain additional test cases (e.g. to demonstrate a failure),
and/or product changes (with bug fixes or features). All product changes should be accompanied by
additional tests to cover and justify the product change unless the product change is strictly an
efficiency improvement and no outwardly observable change is expected.

In the master branch, all tests should always pass. Added tests that fail should be marked as Skip
via `[Fact(Skip = "Test does not pass yet")]` or similar message to keep our test pass rate at 100%.

## Self-service releases for contributors

As soon as you send a pull request, a build is executed and updated NuGet packages
are published to this Package Feed:

    https://ci.appveyor.com/nuget/nerdbank-gitversioning

By adding this URL to your package sources you can immediately install your version
of the NuGet packages to your project. This can be done by adding a nuget.config file
with the following content to the root of your project's repo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <packageSources>
        <add key="Nerdbank.GitVersioning CI" value="https://ci.appveyor.com/nuget/nerdbank-gitversioning" />
    </packageSources>
</configuration>
```

You can then install the package(s) while you have your new "Nerdbank.GitVersioning CI" package source selected:

```powershell
Install-Package Nerdbank.GitVersioning -Pre -Version 0.1.41-g02f355c05d
```

Take care to set the package version such that it exactly matches the AppVeyor build
for your pull request. You can get the version number by reviewing the result of the
validation build for your pull request, clicking ARTIFACTS, and noting the version
of the produced packages.

 [VS]: https://www.visualstudio.com/downloads/
 [NodeJs]: https://nodejs.org
 [NodeJsTools]: https://www.visualstudio.com/vs/node-js/