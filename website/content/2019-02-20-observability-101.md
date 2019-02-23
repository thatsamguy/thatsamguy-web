+++
title = "Observability 101"
date = 2019-02-20
+++
The following post is based upon a previous blog post and talk I gave at [work](https://www.rea-group.com "REA Group") recently.

## Moving beyond monitoring

The concept of "observability" (shortened as "```o11y```") has slowly been gaining traction within the Tech industry recently as an alternative to the existing thought patterns around monitoring.

But what exactly is observability and how is it different?

Observability is quite an interesting concept in my (lovingly biased) opinion. It isn't a new concept nor even a new concept within the Tech industry but instead originates from control theory:

>“In control theory, observability is a measure of how well internal states of a system can be inferred by knowledge of its external outputs. The observability and controllability of a system are mathematical duals.” -  [Wikipedia](https://en.wikipedia.org/wiki/Observability)

That sounds great! (no really!) - but how do we apply this to operating and building software systems?

[Charity Majors](https://twitter.com/mipsytipsy) explains it [like this][4]:

>In the world of software products and services, observability means you can answer any questions about what’s happening on the inside of the system just by observing the outside of the system, without having to ship new code to answer new questions. Observability is what we need our tools to deliver now that system complexity is outpacing our ability to predict what’s going to break.

[Simon Wistow](https://twitter.com/deflatermouse) offers [this explanation][5]:
>Observability is about providing context.
>
>With observability, you can ask new questions about unknowns [...] you should have the data you need to answer those questions (and a clear path to access that data) [...]
>The beauty of observability is that you get an understanding of how a distributed system is working on the inside by reviewing data on the outside.

So what we're really talking about is similar to Business Intelligence (BI), but instead for software systems information.

Critically we need:

* A heap of data about what is happening within our systems
* The ability to easily ask questions about that data and understand the answers
* Lots of context information
* Clear understanding of how that information passed through the system - particularly so for complex distributed systems (woo! microservices!)

## Monitoring != Observability

Isn't this the same as Monitoring? Don't we already collect metrics and logs, search data to find issues and create dashboards to tell us if all is ok?

[Simon Wistow](https://twitter.com/deflatermouse) suggest that [this is not the case][5]:
>Monitoring and logs are no longer enough
>
>Monitoring is the activity of observing the state of a system over time. [...] Monitoring alerts are reactive–they tell you when a known issue has already occurred.
>[...] Which begs an important question, “what about the problems that you didn’t predict?”

Conceptually, monitoring (by some definitions) is generally about gathering data and tracking potential issue that we know how to predict. When we encounter an unknown issue, we attempt to work out the cause, then in many cases add a metric and/or alert to cover that case.

This expands our "known" list of potential issues, increasing the operational maturity of the system, but does not necessarily help cover future unknown issues. And as [Charity says][2]:
>in distributed systems, or in any mature, complex application of scale built by good engineers … **the majority of your questions trend towards the unknown-unknown**.

Generally as an industry, we're getting really good at monitoring these known issues. However, as systems become significantly more distributed and therefore complex, those operating the system spend an increasing amount of time on the *unknown* issues that occur.

Considering this concept of monitoring, [Cindy Sridharan](https://twitter.com/copyconstruct) suggests instead that "Observability" is instead a [superset of Monitoring][1]:
>“Monitoring” is best suited to report the overall health of systems. Aiming to “monitor everything” can prove to be an anti-pattern. Monitoring, as such, is best limited to key business and systems metrics derived from time-series based instrumentation, known failure modes as well as blackbox tests.

>“Observability”, on the other hand, aims to provide highly granular insights into the behavior of systems along with rich context, perfect for debugging purposes. Since it’s still not possible to predict every single failure mode a system could potentially run into or predict every possible way in which a system could misbehave, it becomes important that we build systems that can be debugged armed with evidence and not conjecture.

>“Observability”, according to this definition, is a superset of “monitoring”, providing certain benefits and insights that “monitoring” tools come a cropper at.

![Venn diagram of Observability, Monitoring and Testing](https://cdn-images-1.medium.com/max/1600/1*9BQUrLJoR_SE69Kd8oVlVw.png "Venn diagram of Observability, Monitoring and Testing")

The best part of the superset idea is that we're not starting from scratch. We can and should be monitoring systems, using alerts and centralising our logs and all the best practices that have been built up over time.

What we do need however is a change in culture and thinking to consider the problem from a new perspective - from the top down, instead of bottom up - and allow fresh thinking without the classic restraints and patterns of traditional monitoring practices (E.g.: lack of high cardinality, metrics without context).

Context is everything. Metrics and dashboards can easily show that you are meeting a SLA of 99.95% - but what does this really mean for your customer? Telling your customer that you are meeting your SLA's doesn't change their experience. E.g. The Amazon Web Services status page.

In reality for a 99.95% SLA almost all your customers are working 100% just fine, a few others are effectively working at a 0%-50% SLA level. We need to be able to quickly locate those affected customers/requests and address the issue specific to them.

We then need to know what part of the distributed system is actually causing the issue so that it can be addressed quickly in a few places as possible.

Anyone from Support Agents to Engineers should be able to quickly locate the issue for a customer, identify which part(s) of the system or teams are affecting the issue, and be clear that the issue affects X number of customers in only Y regions on T browser attempting to use S feature with Z impact.

Should we really be having 5 teams investigating a single issue with minimal to no visibility of the data and effects beyond the boundary of their portion of the system?

## Business Value

So "Observability" sounds like a great idea! But what are we trying to achieve by implementing this? What's the point?

From a business perspective:

> * Increased productivity
> * Decreased downtime
> * Happier developers
> * Greater customer satisfaction

Ref: ([Honeycomb][7])

But that sounds like nice piece of fluff.

To achieve this we want anyone (E.g.: Product, Support, Systems and Software Engineers) to be able to ask questions of the systems and get an answer they understand in a quick and timely fashion.

This allows better decisions to be made for a product, shorter feedback loops for engineers developing software, more stable systems and faster debugging of complex issues.

[Charity](https://twitter.com/mipsytipsy) puts this [much more clearly][3]:
>What is the point of observability?
>
>OWNERSHIP.
>
>Observability is about getting the right information at the right time into the hands of the people who have the ability and responsibility to do the right thing. Helping them make better technical and business decisions driven by real data, not guesses, or hunches, or shots in the dark. Time is the most precious resource you have — your own time, your engineering team’s time, your company’s time.

Also on the time to [troubleshoot issues][2]:
>An observable system is one you can fully interrogate. Given a pile of millions of needles, one or two of which have problems, can you slice and dice and sort finely enough to quickly locate literally any given needle?

## Implementation Concepts

To implement observability we need to define and cover a few pillars. Each company has their own way of defining these.

E.g.:
Simon Wistow on [Fastly][5]:

>* Logs
>* Traces
>* Metrics

Cindy Sridharan on [Twitter][1]:
>These are the four pillars of the (Twitter) Observability Engineering team’s charter:
>
>* Monitoring
>* Alerting/visualization
>* Distributed systems tracing infrastructure
>* Log aggregation/analytics

John Fahl on [Logz.io][6]:

> * Monitoring
> * Logging
> * Tracing
> * Analytics
> * Alerting

Generally they can be defined as:

* Monitoring (metrics)
* Tracing (including distributed tracing)
* Analytics/Visualisations
* Alerting
* Logging

In practice for most this means implementing beyond their current monitoring:

* Structured logging - ensuring as must context as possible is included in each event, especially including high cardinality fields
* Distributed tracing - being able to follow a request coming into and out of the system through the many sub-systems owned by multiple teams across a business
* Visualisation/Analytics tooling - bringing all the data together and allowing easy and timely access to the information, no matter the question
* Process change - engineers using the fast feedback loops frequently - build, deploy, observe - to gain understanding of what is normal/abnormal in a system and to ensure that the change had the desired impact quickly

[Edit 2019-02-23] Please also note that if you have sufficiently populated events as your single data source, you can derive all the metrics, lower content logs and alerts all from the event data. See [Charity's recent tweet thread](https://twitter.com/mipsytipsy/status/1098779777136115712) for further info.

## Further reading/listening

Check out the references below as an initial point to give you a great overview of the current thoughts and state of Observability.

If you like podcasts, I'd suggest checking out [```O11ycast```](https://www.heavybit.com/library/podcasts/o11ycast/) by Charity Majors ([@mipsytipsy](https://twitter.com/mipsytipsy)) and Rachel Chalmers ([@rachelchalmers](https://twitter.com/rachelchalmers)).

### References

1. [Monitoring and Observability][1] - Cindy Sridharan ([@copyconstruct](https://twitter.com/copyconstruct))
2. [Observability: What's in a Name?][2] - Charity Majors ([@mipsytipsy](https://twitter.com/mipsytipsy))
3. [Observability: A Manifesto][3] - Charity Majors ([@mipsytipsy](https://twitter.com/mipsytipsy))
4. [Introduction to observability][4] - Honeycomb
5. [Did you see that? Monitoring vs observability][5] - Simon Wistow ([@deflatermouse](https://twitter.com/deflatermouse))
6. [So What is Observability Anyway][6] - John Fahl
7. [How To Talk to Your Boss about Honeycomb][7] - Rachel Perkins
8. [Three Pillars with Zero Answers][8] - Ben Sigelman ([@el_bhs](https://twitter.com/el_bhs))

[1]: https://medium.com/@copyconstruct/monitoring-and-observability-8417d1952e1c "Monitoring and Observability - Cindy Sridharan (@copyconstruct)"
[2]: https://www.honeycomb.io/blog/observability-whats-in-a-name/ "Observability: What's in a Name? - Charity Majors (@mipsytipsy)"
[3]: https://www.honeycomb.io/blog/observability-a-manifesto/ "Observability: A Manifesto - Charity Majors (@mipsytipsy)"
[4]: https://docs.honeycomb.io/thinking-about-observability/intro-to-observability/ "Introduction to observability - Honeycomb.io"
[5]: https://www.fastly.com/blog/monitoring-vs-observability "Did you see that? Monitoring vs observability - Simon Wistow (@deflatermouse)"
[6]: https://logz.io/blog/what-is-observability/ "So What is Observability Anyway - John Fahl"
[7]: https://www.honeycomb.io/blog/how-to-talk-to-your-boss-about-honeycomb/ "How To Talk to Your Boss about Honeycomb - Rachel Perkins"
[8]: https://schd.ws/hosted_files/kccna18/bf/Three%20Pillars%20with%20Zero%20Answers%20-%20A%20New%20Observability%20Scorecard%20%28Kubecon%20Seattle%202018%29.pdf "Three Pillars with Zero Answers - Ben Sigelman"
