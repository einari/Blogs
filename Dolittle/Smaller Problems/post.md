# Smaller Problems

Historically we have been building software products in a monolithic way, meaning that we build the software so that we could release entire products
with all its functionality in one go. This meant a lot of coordination to make sure one could complete features to the specific date set for a release,
even if those features where in completely different parts of the system. Obviously when looking back and the means of distribution was physical media,
you basically had no other choice but to do this - as it would be to much of an effort and too costly to produce the media continuously, not to mention
package and posting it as well. Our industry stood by this approach even when entering the age of the internet - over time we've grown accustomed to
software getting updates on a more regular basis, be it new features, bug fixes or security updates. We would open our software and get presented a dialog
box saying there is an update and when applied you'd have the following new features. The big change is how we approach the development cycles.
Since we no longer need to ship physical media, we can approach the process of delivery differently and allow for a tighter **feedback loop**.
In many cases the approach of how we write the software has not changed. We're dragging behind us the decades of weight of still making monolithic
software. This approach often lead to [tight couplings]() in software and with the principle of [DRY (Don't Repeat Yourself)]() in hand making software
far more generic than it should be. This is where the buzz of [Microservices]() comes in and is supposed to save the day.
Lets dwell a bit on the why here. Why isn't it ok to just continue developing in the way you are. Actually, you might have very good reasons to not
change at all - you might already have it all under control and have a productive team, then there probably is no reason to change. But if you sense
that your software quality is not as great as at it should be, or you're not having the velocity you used to have - you might want to start considering
how you're developing your software. The primary problem one is trying to solve is to break dependencies.
Both on a code level and on an organizational level. Breaking dependencies removes friction; friction to release. You want to have as little friction
as possible to be able to release. If one feature has to wait for another feature before it can be released, you might have an unhealthy friction in
your system. The goal is to become obsessive on releasing, not at any cost; quality matters - but drive your team(s) towards release.
The ideal scenario is that your software with the right boundaries can be released completely independently, without having to think
about the consequences. This would be a benefit even if you have a small team that is maintaining the entire software product. It would mean that
your **time to learning (TTL)** would be smaller - and you'd be iterating more healthier as you would get learn from your software and feedback
continuously, rather than having to wait around. By applying things like the [SOLID principles]() and [Separation of Concerns]() and sticking to
fixing smaller software problems, you can start enabling this scenario and it has the side-effect of making your codebase more sustainable.

## Software as a Service

With the age of the internet and the advent of cloud computing, we've gotten to a point where we don't even have to ship software at all, not even
make it something that you download. With a username and password you can simply use your browser today and log into software and start using it
without installing anything anywhere. For users, this is just great and the way it should be - it removes friction, lowers the barrier to entry
and makes globalization a real opportunity for everyone making software. All of a sudden you also gain more control over which version of the software
is running at any given time, as you have the potential to make your software multi-tenant and basically only have one version running.
This makes it easier to focus on continuously moving forward on delivering business value. However, new challenges needs to be tackled that
previously might not have been on your radar at all - since you're now effectively going into the hosting business with your own software.
Things like performance and scaling for instance. The easy way for traditional monolithic software is to scale on a virtual machine instance level.
With rules tied to specific metrics like CPU load or disk IO and then scale out or in elastically according to usage. With a monolithic software
you're not really doing this motion the most effective as you could; you're scaling everything - while chances are that there is only parts of your
system that needs to be scaled. By breaking up into smaller problems, you gain the opportunity of scaling the pieces individually.
Security ia another aspect that can be dealt with on a more fine-grained level with different security levels for the different parts of the system.

## Bounded Contexts

So, what is a microservice? How big is it? 1000 times larger than a nanoservice? There are different definitions of this in different organizations.
At Dolittle we've had to think hard about this, so that we can provide clear guidance on the topic - as we are a platform to enable the goal and promise
that comes with the buzz of microservices. A microservice is defined as an end-to-end deployable autonomous piece of your software. This means that
it represents all tiers of a vertical slice in your system all the way from its **interaction layer** to the actual data storage.
To find the divide we have come up with the following criteria to look for.

### Ubiquitous language

At Dolittle we are focused around [Doman Driven Design](), expressing a domain using the correct lingo. Within systems one can find multiple
lingo describing different aspects of a concept. This lingo is called the ubiquitous language; the unambiguous language that the domain experts 
representing the aspect talks.In a bounded context, there should only be one representation of a concept.

### Domain Experts

A bounded context should represent a particular group of domain experts. Think of these as the actors in the domain that is being represented.
It does not mean that the actor in the domain is using the software, but might have a representative that represents them in the software.

### Deployment Target

Your software might be available on different devices, in different physical locations. Take for instance software in shipping, parts of it is in
the cloud and parts onboard a ship. Playing different roles for different purposes; one being for the local **edge**, while the cloud part might
be an aggregate of sort - maybe even just reporting of what is on the **edge**.

### Source of truth

There has to be only one unambiguous source of truth and it can't span multiple bounded contexts. In Dolittle this means that a particular state change,
in the form of an event can only be applied from one bounded context. If you're seeing ambiguity in this, you have the wrong division.

### Cohesion

The things that belongs together should go together. We want to remove friction, and keeping the things that belongs together together then makes sense.
You shouldn't be splitting up the software on an arbitrary divide.

## Composing it back together

The last few years we've seen an explosion in popularity with regards to REST APIs - a lot of people claiming its the greatest invention since sliced bread.
REST APIs are nothing more than a well defined approach for connectivity, with a certain level of predictability as to what to expect when using specific
routes and verbs associated with it. REST on its own is nothing more than out-of-process calls from one service to another. The challenge that comes with
composition lies in versioning. Different versions of an API having to be maintained if you want to provide as little dependencies between bounded contexts
as possible. If you drop the complexity of versioning, you've basically brought back the friction and gained nothing but a more fine grained control of
scaling and security. The friction is also on a runtime level, as you have to be able to guarantee the communication. You can put ambassadors in between, both
in process in the form of different retry policies and out of process solutions like [ISTIO](https://www.istio.io).
Sure, your code might be more sustainable within each bounded context - but you've brought back the friction that will make velocity
drop over time. Versioning is a hard problem to solve; how many versions do you keep around and do you have all the implementations hanging around both
in your codebase and at runtime.

At Dolittle we're taking a slightly different path; there are no REST APIs between these things. In order for every bounded context to truly be autonomous,
they have to be enabled for that. Since everything we do is event based and events being the source of truth - these are also the contracts between
bounded contexts. They need to be versioned - or as we call it; have generations to them and mapping rules from one generation to another. This however
is something you don't need to have in your codebase, but only provide to the runtime as metadata for the runtime to deal with when going between bounded contexts.
When processing the events in your bounded context you project to read models that you own in your own data source, for instance a database.
Since everything is broken down into fine-grained events, you'll find it more often than not that you're introducing new events as domains mature in the code
rather than changing old. This is a consequence of new learning in a domain. With every event being stored in an event store forever for each bounded context,
any other bounded context can at their own pace gain this new learning when they're ready by simply consuming the new events.

## Conclusion

Users of software is increasingly becoming more advanced and as a consequence more demanding. In a fast pace world, users are also expecting progression
and problems that might arise to be dealt with as fast as possible. The old approach of releasing a new version of the software once every 12 to 18 months simply
won't work in todays market. You might run the risk of losing business or even worse; go completely out of business. The way you deal with this is to have
faster iterations, without sacrificing quality, so that one can learn faster and meet the market faster. To be able to do this, you need an approach that keeps
your software evergreen. The approach you chose needs to be something that is right for you and your team(s) and can't really be imposed and decided from an
executive level. Dolittle has an approach that we believe in, it is an opinionated approach - well founded in industry accepted patterns, principles and practices.
We also have taken the responsibility of looking at software development more holistically, rather than being a framework for solving one particular problem
we see the benefit of expanding the responsibility so you can [focus on the business value](http://medium.com/dolittle/focusing-on-the-business-value-75d6d2615cf).