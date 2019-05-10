# Release notes; 3.0.0

We just released version 3.0.0 of Dolittle across the board of our major components for .NET.
This post walks you through what has changed and sparked a major version across all the repositories involved.

## Tooling

A major part of building Dolittle applications is tooling and specifically the tooling for generating the metadata regarding your application. Due to our investment in WebAssembly, we needed to improve our build pipeline for MSBuild. We also pay a lot of attention to not being a major bottleneck to the build process in terms influencing the build time too much. **Feedback loops are super important**. 

Since Dolittle is so modularized and decoupled, we needed a core build system that was pluggable in an automatic convention based manner.

This new pipeline is part of the reason for the major version. We have a package called [Dolittle.Build](https://www.nuget.org/packages/Dolittle.Build/) which before 3.0.0 was part of the [Dolittle .NET SDK](https://github.com/dolittle-runtime/dotnet.sdk), which was fine except it broke the naming convention of assemblies coming out. Repurposing the name means a breaking change, so we decided that was the best. This do however have a massive consequence up the stack, since our Runtime is not completely isolated and is in fact also consumed as NuGet packages and the Fundamentals being at the bottom level, you basically have a cascade all the way through.

For any applications this now means you need to have the following package references to get the build going.

```xml
<ItemGroup>
   <PackageReference Include="Dolittle.Build.MSBuild" Version="3.*"/>
   <PackageReference Include="Dolittle.SDK.Build" Version="3.*"/>
</ItemGroup>
```

## WebAssembly

The thing that set in motion the cascade of changes is our [WebAssembly support](https://github.com/dolittle-interaction/WebAssembly). Our support is still very much a work in progress and we do have things that have yet to be implemented and also a bit of work to be left for optimization and improving the development story. But all in all we're very happy with where we are and what we have got thus far. To get started, please read our [getting started guide](https://dolittle.io/interaction/webassembly/getting_started/). We also presented this at the London PWA meetup earlier this week as you can see [here](https://skillsmatter.com/skillscasts/13836-going-enterprise-in-the-browser).

## Dolittle Folder

The Dolittle build tools have a very specific job and that is to generate the metamodel for the application. It holds information about the artifacts and topology of any bounded contexts being compiled. This information is outputted into files that are being used at runtime and located inside a folder called `.dolittle` by default. This is now possible to override so that two projects, for instance a side-by-side scenario of regular application running on a server and a WebAssembly version. These will have two different entry point projects and want to have the same reference to the same projects and share the metamodel.
This is now possible by adding a property to a `PropertyGroup` inside the `.csproj` file:

```xml
<PropertyGroup>
   <DolittleFolder>../Core/.dolittle</DolittleFolder>
</PropertyGroup>
```

## Autofac

Our Autofac support used to live as an [extension](https://github.com/dolittle-extensions) in its own repository. With the effort going into the new build pipeline we had to move this down to [Fundamentals](https://github.com/dolittle-fundamentals/dotnet.fundamentals). The reason being that we wanted to build our build pipeline without compromise on our core principles and wanted to dogfood our own systems for discovery and other things we have in Fundementals. It was moved down, but without any change or coupling to Autofac within Fundamentals and other components. In fact, it is a promise from us not to couple to a specific IoC container within Fundamentals, Runtime, SDK or any other frameworks as we strongly believe that within an application you should be able to chose this yourself. Internally, we hardly ever notice or use anything from the IoC directly, just adhering to the Dependency Inversion principle and trusting the composition will happen.

## Performance Boost

Very important this time around is a general performance boost. This is more a general theme and tiny pieces here and there that has been made faster and more optimal. In total this results in a 

## MongoDB Collections

A breaking change that was introduced in this version is how the naming of collections are generated when using the `IReadModelRepositoryFor<>` interface for read models. This used to be the name of the type of the readmodel, which turns out to be a very bad strategy if you have two different types with the same name representing two different collections only different namespaces. Right now the strategy has been changed to use the fullname of the type but excluding `Read` if the namespace is prefixed with that. In this way it will in fact, due to our features and module naming convention in namespaces, you'll find the naming to be consistent. We do however have an issue registered for [improving on this](https://github.com/dolittle-extensions/ReadModels.MongoDB/issues/26).

