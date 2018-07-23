# Inception

Dogfooding is a fairly common term in software, it speaks about an organization using its own products to build other products.
At Dolittle we've taken this concept to the extreme. Not only are we dogfooding ourselves like we're doing with our frameworks,
but we are at every level reusing what we build to build towards an other level of abstraction or build a specific product, which is
most likely going to be used again. Take for instance our portal; Studio. In order for us to get a new version out, our
continuous improvement pipeline has to build and deploy it. A part of Studio is the continuous improvement pipeline itself,
in order for us to get a new version of it out, it has to build itself. Trust me; it gets confusing real fast.
The benefit of working like this is that we get to try out things in real world scenarios, rather than building things for others
and hoping we're on target. It helps us keep the feedback loop and **time to learning (TTL)** down, so that we are delivering
more accurately with the hope of catching issues even before anyone else does.

## Tenancy

Dolittle offers a hosted experience, the hosted experience is capable of taking any application and build and deploy it.
The goal is that anyone will be able to sign up and get an application up and running within minutes. When you're signing up
you're creating a tenant representing you as a tenant of Dolittle. You're then inside Studio being able to setup your applications.
Studio is another application running in Dolittle under the Dolittle tenant. Again, a full inception is going on.

## Pipelines

## Hosting

## Services