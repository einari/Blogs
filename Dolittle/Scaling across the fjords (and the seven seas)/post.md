# Scaling across the fjords (and the seven seas)

At Dolittle we've set out to make software development more predictable and easier to deliver quality in a continuous manner.
With our frameworks and operational platform on top we aim to take away the biggest infrastructural burdens people building
software has to deal with in order to deliver business value. Not an easy task to take on and do well and keep respecting the
possibilities and choices developers want to do. Taking away freedom is often the price of being able to deliver on such a tough
promise. It is a balance - one we struggle with on a day to day basis; when do we abstract away something - when is the
abstraction too much and you end up creating something way too generic. At the heart of delivering business value sits the
ability to be able for a developer to express the domain they're working in - this is what we focus on. Everything else becomes
secondary when this is the focus. But still, you can't take away all freedom just to do this - its too much of a sacrifice.
As a guiding start; we then lean towards principles and proven software practices that we believe in. I digress.

We're working a lot with the maritime industry and as many other industries, this industry is in the midst of a period of
digitalization. It is one of those buzz-words people talk about, but it has substance behind it. With a history of manual
routines, a lot of paperwork there is a great potential for increased efficiency. On top of this, shipping has a substantial
carbon footprint and [new rules](http://gcaptain.com/shippings-2020-low-sulphur-fuel-rules-explained/) that would add 25% to the
total fuel cost. The numbers for the industry are staggering and opens up opportunities to be more efficient on fuel, but also
other things.

## Software to the rescue?

How could software play a role in this?

For a lot of the digitalization processes its quite obvious that there are some great opportunities for making things more
efficient. With [tens of thousands of vessels](https://www.statista.com/statistics/264024/number-of-merchant-ships-worldwide-by-type/) only in the merchant fleet, you can
quickly see that getting rid of a lot of the paperwork would save a substantial amount for the industry.

New opportunities in using new technology, for instance the [Smart Ropes project](https://www.youtube.com/watch?v=dDQ5PjDRk50) that we've been involved with can also be part of the equation, and also save lives at the same time.

On ships there are a lot of these opportunities, they are already equipped with sensors for quite a few things - but
purposed only for telemetry, not to add intelligence based on the data collected.

If we go back to the fuel element, which is the most costly part of shipping - and quickly becoming more costly. What could
be done to save on fuel and how could software make a difference. Looking at the hull alone, a clean hull has less resistance
in water than a dirty one. Combined with weather - going around bad weather, creating optimized routes could also play a role.
Together with Microsoft, Cisco and Wilhelmsen - these are some of the things we are looking into into a project we've
appropriately named "Occasionally Connected Fuel Efficient Vehicle". This project is to prove architecture and infrastructure
for the marine environment. With vessels not being connected to the cloud all the time, meaning that we need compute power on
the **edge**.

## Connectivity

Even though the vessels aren't connected all the time, you do need to have them connected occasionally. First of all, there are
events that gets generated onboard that you need to transfer to the cloud. Some of these events are coming from IoT scenarios,
and we use the events to train Machine Learning models in the cloud. Other events are coming from consequences of those IoT
scenarios and are translated into **Domain Events** or is in fact generated as a direct consequence of users interacting with
the system. With vessels sailing across vast distances and across the globe, you simply cannot rely on just one type of connectivity.
This boils down to availability and cost. Take something like the typical mobile technologies like GSM/3G/LTE, availability in
near coast scenarios is pretty good across the globe - but the roaming costs can skyrocket through the roof. In port you might
be lucky to have Wifi connectivity. When you don't have these there are other solutions ranging from things like LoRaWan - if
there are vessels connected could form a mesh. Similarly there is narrow-band solutions. Common for both these is low bandwidth.
There is also satellite based solutions and many many more options. Its basically a jungle of solutions.
We need to be completely agnostic towards this and work with all partners, but our software needs to have context awareness
and be able to make decisions based on the type of connectivity available and its cost factor.
The other side of connectivity is our ability to push from the cloud onto the vessel as well.

## Continuously deploying

With our **continuous improvement** pipeline we are guaranteeing that the software will be put in production as fast as possible
based on the deployment strategy configured. This becomes slightly more complex when you have vessels that are occasionally connected
and you never know when they are connected. The complexity lies in versioning the software. With systems that have both a cloud
component and a vessel component and they work together, the complexity increases even further. The ultimate goal is also that
it should all be transparent for the end-user. They only care about doing their job, not version the software is - which is
as it should be. With **edge** deployments, multiple tenants and multiple vessels per tenant - you might need multiple versions
of the software running.

Events, as described in how our [Event Horizon]() system works can be migrated between different versions. This makes it easier to
have one version in the cloud and another on the edge. However, if there are major breaking changes between versions - this might
not be as easy though. There are times when we have to break compatibility, and this should be indicated by the semantic versioning
of the application. Going from a major version of 1.x.x to 2.x.x should be an indication that these versions are very different.

Since everything we do is Docker based and we have complete control over the building of these images, we have the luxury of optimizing
for connectivity. With Dockers layering system, you basically get Docker to pull the difference between versions and it will only
pull what has changed - or the layers that has changed. If you build the Docker images in a good way, you keep the layers small and
only your application code should effectively be pushed across.

## Multi multi multi

The products being built for [Wilhelmsen]() are all multi tenant solutions. This means that the products will offer a possibility
for Wilhelmsens customers to sign-up for as a SaaS solution. Within these solutions you provision software to run and be connected
onboard multiple vessels. These vessels belong to the customer. Every product being built by Wilhelmsen has at least one [bounded
context](smaller problems post), and these are autonomous and they are also segregated by tenant. This means that we're looking at
1 event store per bounded context per tenant and at least 

## Monitoring

