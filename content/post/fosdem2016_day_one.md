+++
subtitle = ""
title = "FOSDEM 2016 Day One"
bigimg = ""
date ="2016-02-05T19:50:58+01:00"

+++

A week ago I went to [FOSDEM 2016](https://fosdem.org/2016/). On the first day, I attended to some talks in the [Python devroom](https://fosdem.org/2016/schedule/track/python/) and in the [Containers and Process Isolation devroom](https://fosdem.org/2016/schedule/track/containers_and_process_isolation/). These are some notes and links from the talks.

<!-- TEASER_END -->

The first three talks belonged the the Python devroom cycle.

Jordi Soucherion ([@jordixou](https://twitter.com/jordixou)) talked about [Python tips, tricks and dark magic](https://github.com/jsoucheiron/pttdm). Nice fast-paced list of quick tricks, specially suited for beginners. Nonetheless, I learned a couple of new tricks, like the fact that `finally` always executes, even if we return in the `except` block.

Yuri Numerov ([@achifaifa](https://twitter.com/achifaifa)) gave a talk on [How to (actually) make games with Python](https://github.com/Achifaifa/slides/blob/master/makegames/makegames.ipynb). A very refreshing short talk about making games without a framework (no [pygame](http://pygame.org/hifi.html) involved). Straight to the minimal definition of a game, the rules, the winning conditions, etc... I encourage you to have a look on the [notebook](https://github.com/Achifaifa/slides/blob/master/makegames/makegames.ipynb) so maybe you'll rethink what a game is.

The next talk was entitled *Why, but why, async and await keywords have been included in Python 3.5* by Ludovic Gasc ([@GMLudo](https://twitter.com/GMLudo)). After a brief review of the concepts of concurrence and parallelism, he explained why he thought that the new keywords, as syntactic sugar for defining and calling coroutines, are better than the previous decorators and  `yield from` syntax. Basically, he points out that it is more newcomer friendly, hides complexity and has same semantics as C#.

Then, I entered the Containers and Process Isolation devroom and stayed there for the rest of the day. The room was fully packed, I had to sit on the floor right next to the speaker in order to attend to the first talk!

Michael Hrivnak ([@michael_hrivnak](https://twitter.com/michael_hrivnak)) talked about [Docker for Developers](https://fosdem.org/2016/schedule/event/docker_for_developers/attachments/slides/995/export/events/attachments/docker_for_developers/slides/995/docker_for_devs_short.pdf). The focus of the talk was the scenario in which you organization does not use Docker but you use it for your own development and testing of applications in a consistent way. Good introduction to Docker for this use case.

Ray Tsang ([@saturnism](https://twitter.com/saturnism)) spoke about *Scaling with Kubernetes, Automatically!*. Ray is a developer advocate at Google and demoed us how a Kubernetes cluster of [infinispan](http://infinispan.org/) containers can grow dynamically via the `kubectl scale` command. I think he used this [tool](https://github.com/saturnism/gcp-live-k8s-visualizer) to make it more visually appealing. He also had a cluster of Raspberry Pi running a cluster of Kubernetes, which he used in another talk that he gave. The how-to of the making of the cluster can be found on a series of posts on [the Kubernetes blog](http://blog.kubernetes.io/2015/11/creating-a-Raspberry-Pi-cluster-running-Kubernetes-the-shopping-list-Part-1.html).

Aditya Patawari ([@adiyapatawari](https://twitter.com/adiyapatawari)) presented his talk about *Fault Tolerance with Kubernetes*. Well, more than a talk, it was a hands-on session based on a [simple example](https://github.com/adimania/kubernetes-example) of a replication controller managing a pod, and seeing in action how it reacts to killing a pod.

There was also a very interesing talk of [Ian Downes](https://www.linkedin.com/in/ndwns) about *Powering Twitter's infrastructure with containers*. He said that the compute nodes of Twitter are based on a  [Mesos](http://mesos.apache.org/) + [Aurora](http://aurora.apache.org/) architecture, and that the [Completely Fair Schedule](https://en.wikipedia.org/wiki/Completely_Fair_Scheduler) of the Linux kernel is not the best option for every process because it does not know how much of the work the thread has accomplished.

[David Drysdale](https://github.com/daviddrysdale) spoke about [Capsicum: Capability-based sandboxing](https://fosdem.org/2016/schedule/event/capsicum/attachments/slides/920/export/events/attachments/capsicum/slides/920/CapsicumSlides.pdf). This was not a talk about containers but process isolation. It was about [Capsicum](https://www.cl.cam.ac.uk/research/security/capsicum/), a framework that aims to define security rules both in a fine-grained and simple approach.

Finally, Maciej Pasternacki ([@mpasternacki](https://twitter.com/mpasternacki)) talked about [Jetpack: a container runtime for FreeBSD](https://fosdem.org/2016/schedule/event/jetpack/attachments/slides/1155/export/events/attachments/jetpack/slides/1155/FOSDEM2016_jetpack.pdf). A bright and funny talk about an on-progress alternative to containers on FreeBSD, based upon the appc/spec container specification, ZFS for layered infrastructure, jails for pod management, a fat binary written in Go (and no services involved) and Makefiles instead of Dockerfiles. Not production-ready but a very interesting approach that shares the good engineering practices and slow but steady approach that FreeBSD is proud of. Brilliant!
