# daten-lokator-demo

Represents an example of how the [Daten Lokator][DatenLokator] library can be used to automatically retrieve test data.

The sample consumes the published [BlueDotBrigade.DatenLokator][NuGetPackage] NuGet package, exactly as any other project would. The library's own source code lives in the [daten-lokator][DatenLokator] repository.

## Overview

| Project | Purpose |
|---|---|
| [`Src/Demo`](Src/Demo) | A trivial application: droids, a serializer, and an inventory pipeline that calls a web API. |
| [`Tst/DemoTests`](Tst/DemoTests) | MSTest tests for the application, showing how Daten Lokator reduces visual noise by loading test data with a single line of code. |

The tests in [`XmlSerializerTests.cs`](Tst/DemoTests/Serialization/XmlSerializerTests.cs) compare a conventional test (an embedded multi-line string) with tests that load their expected data from files:

| Test | Daten Lokator feature | Input data |
|---|---|---|
| `ToXml_ProtocolDroid_StringsMatch` | File selected by naming convention (`Member_Case_Result`) | `.Daten/Serialization/XmlSerializerTests/ProtocolDroid.xml` |
| `ToXml_RudeProtocolDroid_StringsMatch` | File selected by name | `.Daten/Serialization/XmlSerializerTests/Bespin.xml` |
| `ToXml_PitDroid_StringsMatch` | Shared data from the global folder | `.Daten/.Global/PitDroid.xml` |
| `ToXml_SuperBattleDroid_StringsMatch` | Automatic decompression of `*.zip` files | `.Daten/Serialization/XmlSerializerTests.zip` |

[`DroidInventoryPipelineTests.cs`](Tst/DemoTests/DroidInventoryPipelineTests.cs) shows `Lokator.Register(...)`, which serves static content from a lightweight in-process endpoint so an HTTP client can be tested without an external server.

[`TestEnvironment.cs`](Tst/DemoTests/TestEnvironment.cs) shows the one-time setup (`Lokator.Get().Setup()`) in an MSTest `[AssemblyInitialize]` method.

## Getting started

Install the .NET 9 SDK (or later), then:

```bash
dotnet test DatenLokator-Demo.sln
```

The `BlueDotBrigade.DatenLokator` package is restored from nuget.org; no other setup is needed.

## Repository layout

| Path | Contents |
|---|---|
| `Src/Demo` | The sample application. |
| `Tst/DemoTests` | Tests and their `.Daten` input data. |
| `DatenLokator-Demo.sln` | Solution. |

When the tests run, Daten Lokator extracts `*.zip` input files into temporary `*~ZIP` folders beside them; these are ignored by Git. The zip file is stored as a normal file (not in Git LFS), so cloning this repository does not require Git LFS.

[DatenLokator]: https://github.com/BlueDotBrigade/daten-lokator
[NuGetPackage]: https://www.nuget.org/packages/BlueDotBrigade.DatenLokator
