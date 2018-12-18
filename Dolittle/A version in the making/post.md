
# A version in the making

Its been almost a year since we founded Dolittle. During that period we've been hard at work getting the next official release of our platform out; 2.0.0. Yesterday we finally hit the release button and got it out it. It's been longer in the making though; almost 2 years with the pre-release tag of **alpha**.

CAKE IMAGE

## Where is version 1.x?

"How did we reach version 2 level without going through a version 1?", you might be asking. Well, Dolittle has a legacy that has been close to 10 years in the making called [Bifrost](https://github.com/dolittle/bifrost). 2 years ago we decided to re-position the product and evolve it towards becoming a true microservice oriented solution. We refer to Bifrost as version 1 and it felt natural to start Dolittle off with version 2. 

There is also another hidden gem in why 2 makes sense; this is the second time around with Dolittle as a company. The context and approach was different, but we had Bifrost as a centerpiece in version 1 of the company. That's a story for another time, though.

## Versioning

Versioning software can be very hard. And for a public facing framework, it's one of *the* hardest thing one can deal with, in our experience. The reason this is hard is due to dependencies; what will break from changes and how do we communicate that well and also make sure we are consistent. 

Our philosophy is to have an **always moving forward** type of system. This implies not looking back and patching older versions. With API's that are at the very core of other software products, this becomes even harder and you have to be much more careful.

[Semantic Versioning](https://semver.org) is the key to everything we do moving forward. We've been adhering to it for the last 2 years; but it was easy to adhere to it - because we had the **alpha** tag :). Moving forward, we will put forward the discipline needed to have predictable versions. We've written it all down [here](https://dolittle.io/general/versioning/).

## Exploded software

Our legacy; [Bifrost](https://github.com/dolittle/bifrost) was for all intents and purposes a single .NET assembly. Sure, we had a couple more - but the core runtime, SDK and all was in one package. This was one of the things we wanted to address 
working on version 2. With the single package, we had created a lot of friction in the codebase by having couplings that were almost impossible to break. We call it friction because we started having problems with flexibility and changeability. At Dolittle we're very focused on building high quality software that embraces change and adheres to a set of principles, such as [SOLID](), [Separation of Concerns](). 

To put ourselves in the pit of success for the next phase of the platform, we had to explode the single assembly and see where the pieces landed.

The single assembly became the following:

| Repository | # of packages | Description |
| ---------- | ------------- | ----------- |
| Fundamentals | 37 | Fundamental reusble building blocks |
| Runtime | 18 | Our core microservice, CQRS, domain driven engine |
| C# SDK | 19 | The C# SDK for working with the Runtime |

In addition to this, we've introduced the following core components:

| Repository | # of packages | Description |
| ---------- | ------------- | ----------- |
| AspNetCore | 6 | Core functionality when working in an ASP .NET Core app |
| Autofac | 1 | Specific support for Autofac as the IoC container |
| MongoDB ReadModels | 1 | Support for MongoDB readmodels |
| MongoDB EventStore | 1 | Development purposed eventstore |
| Azure EventStore | 1 | Leveraging Azure CosmosDB as the eventstore |

One of the goals was to change our approach in the frontend. We had [our own Single Page Application framework](https://github.com/dolittle/Bifrost/tree/master/Source/Bifrost.JavaScript) built on top of [Knockout](https://knockoutjs.com).

With the fragmented world in JavaScript and Web, we decided early on to be framework agnostic and support whatever a project needs. This means we had to decide where to put what. Everything that is general across the different frameworks had to be in one place and everything specific in its own. 

We knew we wouldn't have the capacity to deal with all frameworks at the same time and with a love for [MVVM]() / [Presentation Model](); [Aurelia](http://aurelia.io) was the framework we wanted to use for our own applications, such as [the Studio](https://dolittle.studio) we're in the midst of building.

Also, an important piece of the puzzle is to make everything look good - we decided early on to tackle this as well - instead of using existing frameworks for styling that was out there.

This gave us then the following packakges:

| Repository | # of packages | Description |
| ---------- | ------------- | ----------- |
| Dolittle Styles | 1 | Our styles framework |
| Common Client | 4 | Common client functionality |
| Aurelia Client | 1 | Aurelia specific client support |
| Aurelia Components | 1 | Reusable Aurelia web components |

On top of all this we also have our tooling we're working on:

| Repository | # of packages | Description |
| ---------- | ------------- | ----------- |
| CLI | 1 | Our Command Line Interface for everything Dolittle |
| VSCode | 1 | Our Visual Studio Code extension |

In addition to all this we're also heads down working on solutions for Digital Twins,
WebAssembly support and more.

The purpose of the explosion is to be able to version each repository and provide a way to work in isolation for each of them. This allows for each repository to have independent release cadences and changed without worrying about the dependency load of everything consuming it. This is where SemVer and abiding strictly to it comes in for us. You can find all our NuGet packages [here](https://www.nuget.org/packages?q=dolittle)
and NPM paackages [here](https://www.npmjs.com/search?q=dolittle).

## How did we know it was time to release?
Obviously, close to 2 years in **alpha** is a dead giveaway that we should have released a major release quite some time ago. But it really wasn't that clear cut for us. Even though we strive towards releasing as early as we can, so that we can understand more and continuously improve from it -
we wanted to have a certain level of maturity of our APIs before we called it 2.0.0.

From working directly with customers, such as the [Red Cross CBS project](http://github.com/ifRCGo/cbs) and others - we've stabilized the platform and APIs in the **alpha** period.

## Cascades of versions

We discovered that applications with a top-layer dependency to Dolittle struggled to update patches and minor releases of their dependencies. When using [NuGet]() for our packages, and even though the intermediate packages are taking wildcard dependencies on minor, the top layer won't get changes from the bottom automatically. This comes down to how .NET Core does its bindings internally when building packages. In order for us guarantee that you get bug and security fixes from the bottom layers, we had to cascade build everything. This is now something we're building into our own build routines to be able todo this.

## Moving forward
Our universe is constantly expanding; more packages, more versions and more deployables in general. We will do our best to make the journey for anyone using Dolittle as predictable as possible through versioning and also changelogs. Roadmaps are also something we're trying to become better at - we need a way to express this in a way that matches how we work. We're still trying to discover this. Look closely at this place and any of our social media streams (links....).
