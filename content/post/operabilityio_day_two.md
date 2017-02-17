+++
subtitle = ""
title = "Operability.io - Day two"
bigimg = ""
date ="2015-09-25T20:37:26+02:00"

+++

The second and last day of [operability.io](http://operability.io) has been packed of interesting talks. In the following paragraphs I'll pick some details about each talk.

<!-- TEASER_END -->

**Charity Majors** ([@mipsytipsy](https://twitter.com/mipsytipsy)) has talked about *Building a World Class Ops Teams*. Very valuable thoughts as:

- If you are a startup, you need an ops team only if you need extreme reliability, scalability, security or you are tackling with operational category problems for the whole internet. Otherwise, stick with your lean dev team.
- Things that predict a **good ops** candidate: strong automation skills, ownership of their systems, strong opinions weakly held (yeah, I think that this is a characteristic of excellent professionals anywhere), love to simplify, good communication skills, calm in crisis, respect to processes, empathy.
- Things that do **NOT** predict a good ops candidate: whiteboard coding exercises, degrees, knowledge in any particular technology, big company pedigree.
- In order to hire: make good questions that probe the candidate's self-reported strengths, related to your real problems. Look for strengths, not for lack of weaknesses.
- Things that are **dangerous** in ops engineers: tweaking indefinitely, adding complexity, walling off production environment from developers, refusal to admit that they don't know things, disconnection from customer experience.
- If you want to **retain** good ops engineers, treat them well, empower them and give them interesting and motivating problems to solve.
- Finally, on my view, a very sharp description of the profile: *A good operations engineer is broadly literate and can go deep on at least one or two things*

**David Mytton** ([@davidmytton](https://twitter.com/davidmytton)) has told us how they deal with incidents at [Server Density](https://www.serverdensity.com). Also great advice:

- Document any process involving troubleshooting using an easily **text searchable** system. Put the docs out of your infrastructure, so that in case of emergency they will be accessible.
- Log every action you take.
- Simulation (wargames) can be very didactic exercises.
- Make **checklists** to mitigate effects of fatigue, stress, complexity and ego.
- Make frequent public updates when dealing with the incident.
- Gather the team and escalate when necessary.
- Make honest and constructive *post-mortems*.

**Emma Jane** ([@emmajanehw](https://twitter.com/emmajanehw)) discussed about empathy and the importance of this skill.

**Scott Klein** ([@scootklein](https://twitter.com/scootklein)), from [StatusPage](http://statuspage.io) told us what make a good status page:

- Timestamps, preferably relative or translatable.
- Mobile-friendly.
- Out of your infrastructure, and even served by another DNS provider.
- With clear indications on how to contact.

Some good advice on what to do on status reports: communicate precisely and often, set expectations of the update frequency, apologize, don't give resolution ETAs, take responsibility and don't blame providers, be personal.

And I have learnt about the [Service Recovery Paradox](https://en.wikipedia.org/wiki/Service_recovery_paradox)

**Rich Archbold** ([@rich_archbold](https://twitter.com/rich_archbold)) has spoken about how establishing values at [Intercom](https://www.intercom.io) led to better performance. The most remarkable of their values, from my point of view, are:

- Zero touch ops (avoid manual interventions)
- Run less on premise software (more SaaS)

**Matthew Skelton** ([@matthewpskelton](https://twitter.com/matthewpskelton)) did a very interesing and amusing talk about good and bad loggins practices. Besides from making analogies of maritimal logging and software logging, he has given many juicy pieces of advice such as:

- Log continuously.
- Use `enum` or other unique and readable identifiers for event states logging.
- Do not hardcode event severity, map it using the event identifiers at runtime.
- Use unique IDs to track logs flowing through layers.
- Use NTP to get reliable timestamps in the logs.

And I have discovered the concept of [Structured Logging](http://www.sunitparekh.in/posts/structured-logging).

**Gareth Rushgrove** ([@garethr](https://twitter.com/garethr)) gave an amazing talk about the evolution of OSs from their conception to current state of the art, and introducing the concept of [Microkernel](https://en.wikipedia.org/wiki/Microkernel) (a library operating system) that allows compiling apps into a kernel, only including the capabilities that you need.

For me, it sounded really disruptive. Now, everything is an application, excepting the hypervisor. It will result on these advantages:

- Hypervisor / hardware isolation.
- Smaller attack surface area.
- Running less code.
- Enforce immutability.
- No default remote access.

Paraphrasing Gareth: *Microkernels are the promise of containers but without needing to pretend the intermediary OS doesn't exist*. Hands down :-)

**Ben Hughes** ([@benjammingh](https://twitter.com/benjammingh)) closed the conference with an introduction to security. I have learnt that:

- [Gitrob](https://github.com/michenriksen/gitrob) is a great tool to identify leak of information in Git repositories.
- Attackers are already mining any public info you have (Github, S3, ...).
- Security bug bounty programs scale and are better that security consultancies.
- Don't use the dangerous `curl | bash` installation pattern (well, I knew that one). He even demoed and exploit!
- Don't trust Vagrant boxes from not verified URLs.
- Don't run containers as root, as sometimes root can escape the container.

So, finally, I left the conference with some new tools and tricks on my backpack.
