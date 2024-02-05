# FOSDEM 2024

I attended [FOSDEM 2024](https://fosdem.org/2024/) in Bruxelles (Brussels) on Sat/Sun Feb 3/4, 2024.

The [full agenda](https://fosdem.org/2021/schedule/) shows there again was a lot of content, as at many big tech conference. Below are notes about a few session I listened in to. Do have a look around the schedule and check out the slides and recordings of other sessions in your own areas of interest!

This must have been I think my 4th or 5th time attending this _"event for free software developers to meet, share ideas and collaborate"._
Looks like I [last blogged in 2021](FOSDEM-2021.md) (but I seem to have been too lazy in 2023 and 2019 & 2018).

For the first time, I traveled from Switzerland to Belgium via Paris by train, instead of by plane.
I'm not entirely sure anymore if it's not already too late for this climate gesture to save the 🐧 and the 🐢, but it still seemed worth giving it a shot.
(Sadly travel by train was more expensive than flying - talk about upside down economic incentives!)

## Friday

On Friday late afternoon as I arrived, I peeked in to the [EU Open Source Policy Summit 2024](https://summit.openforumeurope.org) (which I attended last year),
where I ran into Swiss member of federal parliament [Matthias Stürmer](https://www.google.com/search?gs_ssp=eJzj4tVP1zc0zDBMKUixNIs3YPQSzE0sKcnITCxWKC45vKcoN7UIALgEDAE&q=matthias+st%C3%BCrmer&oq=matthias+st%C3%BCrmer&gs_lcrp=EgZjaHJvbWUqBwgBEC4YgAQyCggAEAAY4wIYgAQyBwgBEC4YgAQyBggCEAAYHjIGCAMQABgeMgYIBBAAGB4yBggFEAAYHjIGCAYQABgeMgYIBxAAGB4yCAgIEAAYBRgeMgYICRAAGB7SAQg1MTc0ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8&si=AKbGX_p4pyeyr1FGV3SWZkER8f4E6kGHgT4D1VdN1tYTxAlPK7_jEoIpRlHbY0TgEMXW0WV1-tEjeO5Ie0MJ4HB_B7AsQtM1p6ecioy8232E9UNIRJ7Pop4%3D&ictx=1&ved=2ahUKEwiOo42hhI2EAxWQhf0HHeHACY8QyNoBKAB6BAgaEAA).
We had a very interesting discussion about open source and its sustainability, digital sovereignty, AI, and Open Linked Data. He gave at talk at FOSDEM the following day - see below.

Friday evening I met up with Kai (Mr. openHAB) - because what else are Tech Conferences really good for, if not to meet up with your old friends!

## Saturday

* [Bridging Research and Open Source: The genesis of Gephi Lite](https://fosdem.org/2024/schedule/event/fosdem-2024-3253-bridging-research-and-open-source-the-genesis-of-gephi-lite/) presented [Gephi Lite](https://gephi.org/gephi-lite/) (on [gephi/gephi-lite](https://github.com/gephi/gephi-lite)), a Browser-based Graph visualization tool. It's the successor of the originally Desktop Java-based [Gephi](https://gephi.org) Graph Viz Platform  (`*.gexf`), and (I think) [Retina](https://ouestware.gitlab.io/retina/) and [Graphology](https://graphology.github.io) + [sigma.js](https://www.sigmajs.org).

* [Cosma, a visualization tool for network synthesis](https://fosdem.org/2024/schedule/event/fosdem-2024-3394-cosma-a-visualization-tool-for-network-synthesis/) presented [Cosma](https://cosma.arthurperret.fr), a cool [Personal Knowledge Management tool](https://docs.enola.dev/concepts/other/#personal). (Also if I'm ever near Mons in Belgium, I'll visit the [Mundaneum museum](http://www.mundaneum.org) about [Paul Otlet](https://www.google.com/search?gs_ssp=eJzj4tTP1TcwNTKpSDJg9OIqSCzNUcgvyUktAQBLdwcD&q=paul+otlet&oq=paul+otl&gs_lcrp=EgZjaHJvbWUqBwgBEC4YgAQyBggAEEUYOTIHCAEQLhiABDIHCAIQABiABDIHCAMQABiABDIHCAQQABiABDIICAUQABgWGB4yCAgGEAAYFhgeMggIBxAAGBYYHjIICAgQABgWGB7SAQgzMzUyajBqN6gCALACAA&sourceid=chrome&ie=UTF-8).)

* [From the lab to Jupyter : a brief history of computational notebooks from a STS perspective](https://fosdem.org/2024/schedule/event/fosdem-2024-3190-from-the-lab-to-jupyter-a-brief-history-of-computational-notebooks-from-a-sts-perspective/) argued that iPhyton Notebooks (AKA Jupyter, also used in [Google's Colabs](https://https://colab.google/)) is as popular as it is today because it sneaked its way into that popularity by being taught to a generation of students at universities, who subsequently "brought it with them" into industry.

* [Java 23 Foreign Function Memory API](https://fosdem.org/2024/schedule/event/fosdem-2024-1714-foreign-function-memory-api/) presented Panama JEP 454 & 460, the "new JNI". The room chuckled when an attendee corrected the speaker that JNI cannot address 2 GB [but 1 byte less](https://xkcd.com/2248/)... ;-)

* [RDF Dataset Canonicalization: Scalable security for Linked Data](https://fosdem.org/2024/schedule/event/fosdem-2024-2719-rdf-dataset-canonicalization-scalable-security-for-linked-data/) listed examples of huge RDF datasets, incl. (of course) [Wikidata's 1500 million RDF statements](https://www.wikidata.org), Musicbrainz and [the EU's](https://data.europe.eu). It then mentioned [the W3C's RDF Dataset Canonicalization TR](https://www.w3.org/TR/rdf-canon/), and [W3C Verifiable credentials](https://en.wikipedia.org/wiki/Verifiable_credentials) - which, according to this speaker, will _"likely be the basis for EU personal ID cards"._ (I was attending this session because I recently started reading up reading more about this overall space, in the context of a new side project I'm exploring at [Enola.dev](https://docs.enola.dev).)

* [The new Swiss Open Source Law: "Public Money Public Code" by default](https://fosdem.org/2024/schedule/event/fosdem-2024-3401-the-new-swiss-open-source-law-public-money-public-code-by-default/) was really interesting - apparently Switzerland has a new law, possibly the first of its kind in the world (?), called _"[EMBAG](https://www.admin.ch/gov/de/start/dokumentation/medienmitteilungen.msg-id-98828.html)",_ which went into force just now, in January 2024. It mandates that all software developped by and for the Swiss federal administration has to be released as open source - that's huge! I wonder if the policitians who voted in favour of this really understood the full implications of this? ;-)

  They also mentioned https://ossbenchmark.com, which a fun site which does data mining about open source contributions by some Swiss companies. (It would fun to do this kind of thing more broadly? Think and collect Git data globally, but still interpret and act on it locally. IDK if e.g. GH/LF/GOSPO do things like this?)

Between talks I ran into some old friends in hallways - notably Zaheda and Dave Neary, with who I chat about [Mifos](https://www.mifos.org) (and ARM, and ML - always great to re-connect).

## Sunday

* Sunday morning started with a talk learning about the facinating breakthrough Internet protocols [HTTP 418](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/418) and [RFC 1149](https://datatracker.ietf.org/doc/html/rfc1149) (with [Save 418](https://save418.com), and [google.com/teapot](https://www.google.com/teapot))... 😆

* [You too could have made curl!](https://fosdem.org/2024/schedule/event/fosdem-2024-1931-you-too-could-have-made-curl-/) by the [Daniel Stenberg](https://daniel.haxx.se) cURL: _"My project, well **our** project..."_ / _"Maybe roughly 20 billion installations (but hard to tell)"_ / _"cURL is known to run on 2 planets."_ / _"Gold medal from my king, for 'created economic value which cannot be overstated'."_ / _"Start small"_ / _"Give it time."_ / _"Put in the time! (You'll have to compromise on other things.)"_ / _"Wow, 300 downloads!"_ / _"Keep working on it"_ / _"Make it easy for others to help"_ / _"Learn to use the time you get & learn to do fewer other things"_ / _"Give it more time"_ / _"Hard work and endurance"_ / _"We're all competing for the same pool of open source developers."_ / _"Own your mistakes."_ / _"Keep having fun!"_ / _"Everyone makes mistakes - how to act on them is what matters. Take care of the people who make makes (usually myself)"._ / _"People are more difficult than code."_ / A surprisingly many car owners support questions! (People find his email in the cURL license text that's... basically everywhere.) But also: _"$SUBJECT: 'I will slaughter you."_ crazies. / _"It's fine to have phases of more or less motivation, accept it, embrace it, work on another part."_ / _"Imposter syndrome is real."_ / _"If not done today, continue tomorrow. It's fine."_ / _"Realize, and be grateful for, that having spare time for hacking is a lucky luxury privilege."_ / _"Do not split your attention on 417 projects."_ / _"Release early, release often"_ / _"Reduce contributor friction"_ / _"Endure."_ / _"I wanted a tool for..."_ (AKA _"Scratch your own itch.")_ Again: _"Have fun!"_ Moar such thoughts on his https://un.curl.dev.

* [OpenTelemetry Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/) provide a Common Schema for Observability.

* [SCION](https://scion-architecture.net) (on [GitHub](https://github.com/scionproto/scion)), a clean re-design of a new Internet architecture. Designed for security, as an alternative to BGP/IP. From ETH in Zürich, Switzerland. Has ~100+ ASes at a few international ISPs connected. Notably underlying the Secure Swiss Finance Network (SSFN), which phases out previous Finance IPNet by June 2024, and the Secure Swiss Health Network (SSHN). Clients can put constraints on routes, and e.g. exclude some AS or entire "domains" (which they typically map to jurisdictions, apparently). AWS offers SCION access already, and there are Browser extensions, and client libraries in Go, Rust, Java and other bindings. It all sounded interesting - but... a change of this order of magnitude is (very) ambitious - just think IPv6. How realistic is it for this to gain any sufficiently relevant adoption, outside of niche private networks, à la SSFN/SSHN? What value does full AS exclusion really add that you in practice don't similarly already get with a classic encrypting overlay VPN network? Is this unique, or does every network research group build one of these? (Just like every CS departement really should come up with a new prog lang, for some self respect?) IDK.

* [Version control post-Git](https://fosdem.org/2024/schedule/event/fosdem-2024-3423-version-control-post-git/) presented [Pijul](https://pijul.org) is a new Version Control System (VCS). It uses Operational Transforms (OT), and smart math, which makes it much better than Git... more intelligent conflict handling, no need for Git LFS like hacks, and much more scalable to huge mono repos (or so the speaker claimed; I know of a certain one at work that could be fun to try this with). I thought it looks promising.

Between talks I chat with Nicolas, a (no more!) "friendly stranger" that was at the dinner with Kai, Matthias and Rika yesterday evening. He lives in a city nearby home, and works in embedded systems - and helped me better understand what the [Device Tree_ in Linux for ARM](https://docs.kernel.org/devicetree/usage-model.html) was all about - Thank You!

## Booths

I didn't spend much time visiting booths this year, and added only 1 new sticker to the laptop... ;-)

[Minetest](https://www.minetest.net) had a booth, and is really cool!
