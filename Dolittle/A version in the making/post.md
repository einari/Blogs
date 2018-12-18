
# A version in the making

Its been almost a year since we founded Dolittle. During that period we've been hard at work
getting the next version of our platform out; 2.0.0. We finally hit the release button and
got it out it. Its been even longer, almost another year that the versions coming out has
been marked with the pre-release tag of **alpha**.

## Where is version 1.x?

How did we reach version 2 level without going through a version 1 you might be asking.
Well; Dolittle has a legacy that has been close to 10 years in the making, called [Bifrost](https://github.com/dolittle/bifrost).
2 years ago we decided to rename the project and position it differently and also grow it
towards becoming a true microservice oriented solution. The legacy is considered the version
1 of Dolittle and to us it felt natural to call it a version 2. There is also another hidden
gem in why 2 makes sense; this is the second time around with Dolittle as a company.
Although with a very different context and approach this time around, we had Bifrost as a
centerpiece also in version 1 of the company.

## Versioning

Versioning software is very hard, and in our opinion probably *the* hardest thing one can
deal with. The reason this is hard is due to dependencies; what will break from changes
and how do we communicate that well and also make sure we are consistent. In a **always moving forward**
type of system where one does not look back and patch older versions, one has to be
very careful with this. With APIs that are going to be used by other software, this becomes
even harder and you have to be much more careful.

[Semantic Versioning](https://semver.org) will be the key to everything we do moving forward.
We've been adhering to it for the last 2 years; but it was easy to adhere to it - because
we had the **alpha** tag :). Moving forward, we will put forward the discipline needed to
have predictable versions. We've written it all down [here](https://dolittle.io/general/versioning/).

## Exploded software

Our legacy; [Bifrost](https://github.com/dolittle/bifrost) was for all intents and purposes a single
.NET assembly. Sure, we had a couple more - but the core runtime, SDK and all was in one package.
This was one of the things we wanted to address when working on version 2. With the single package
we had, we had basically unintentionally created a lot of friction in the codebase by having couplings
that was close to impossible to break. We call it friction because we started having problems with
flexibility and changeability. At Dolittle we're very focused on building high quality software adhering
to a set of principles, such as [SOLID](), [Separation of Concerns]() and in general embracing change.
To put ourselves in the pit of success for the future, we had to explode the single assembly and
see where the pieces land.

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

One of the goals we also had was to attack the frontend differently. We originally had [our
own Single Page Application framework](https://github.com/dolittle/Bifrost/tree/master/Source/Bifrost.JavaScript) built on top of [Knockout](https://knockoutjs.com).
With a fragmented world in JavaScript and Web, we decided early on to be agnostic and support
whatever needs to be supported. This means we had to decide where to put what.
Everything that is general across the different frameworks had to in one place and everything
specific in its own. We knew we wouldn't have the capacity to deal with all frameworks at the same time
and with a love for [MVVM]() / [Presentation Model](); [Aurelia](http://aurelia.io) was
the framework we wanted to use for our own applications, such as [the Studio](https://dolittle.studio) we're in the midst of building.
Also, an important piece of the puzzle is to make everything look good - we decided early on
to tackle this as well - instead of using existing frameworks for styling that was out there.

This gave us then the following packakges:

| Repository | # of packages | Description |
| ---------- | ------------- | ----------- |
| Dolittle Styles | 1 | Our styles framework |
| Common Client | 4 | Common client functionality |
| Aurelia Client | 1 | Aurelia specific client support |
| Aurelia Components | 1 | Reusable Aurelia web components |

On top of all this we also have our tooling we're working on:

| Repository | # of packages | Description |
| ---------- | ------------- | ----------- |
| CLI | 1 | Our Command Line Interface for everything Dolittle |
| VSCode | 1 | Our Visual Studio Code extension |

In addition to all this we're also heads down working on solutions for Digital Twins,
WebAssembly support and more.

The purpose of the explosion is to be able to version each repository individually and
provide a way to work in isolation for each of them and have independent release cadences
and improve each repository as fast as they can without having to worry about the
dependency load of everything consuming it. This is where SemVer and abiding strictly to it
comes in for us.

## How did we know it was time to release?

Obviously, close to 2 years in **alpha** is a dead giveaway that we should have released a major
release quite some time ago. But it really wasn't that clear cut for us. Even though we strive towards
releasing as early as we can, so that we can understand more and continuously improve from it -
we wanted to have a certain level of maturity of our APIs before we called it 2.0.0.
From working directly with customers, such as the [Red Cross CBS project](http://github.com/ifRCGo/cbs) and
others - we've stabilized the platform and APIs in the **alpha** period.

## Cascades of versions

One of the discoveries we've done with regards to versioning is that when working in a separated
way like this, and when you want to bubble up minor and patch releases - it becomes hard when the
applications leveraging Dolittle is taking a dependency to the top layer. We found that at least when
using [NuGet]() for our packages, and even though the intermediate packages in between are taking wildcard
dependencies on minor - the top layer won't get changes from the bottom automatically. This comes down
to how .NET Core does its bindings internally when building packages. In order for us
guarantee that you get bug and security fixes from the bottom layers, we had to
cascade build everything. This is now something we're building into our own build routines to be able to
do this.
