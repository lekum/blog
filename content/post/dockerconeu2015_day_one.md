+++
subtitle = ""
title = "Dockercon EU 2015 Day One"
bigimg = ""
date ="2015-11-17T19:30:49+01:00"

+++

I've just arrived from [Dockercon EU 2015](https://europe-2015.dockercon.com) and it has been a really interesting thematic conference. This is the first post of a series of two posts, in which I will share some picks and links of the talks I have attended to.

<!-- TEASER_END -->

The **General session** was performed by Ben Golub ([@golubbe](https://twitter.com/golubbe)), Solomon Hykes ([@solomonstre](https://twitter.com/solomonstre)) and Aanand Prasad ([@aanandprasad](https://twitter.com/aanandprasad)), among others. It gave a high-level view about news in the Docker ecosystem. It also set the general theme of the conference, based upon three concepts:

1. Empower the makers
2. Docker in production
3. End-to-end solutions

They described the docker stack, which is comprised of *solutions*, *dev tools*, *infrastructure* and *standards*, with fantastic cartoon-style designs (Docker designers really rock!).

Some news:

- Docker Compose supports all new Swarm / Engine features. Thus, this tool is the one *officially* supported to define and manage your infrastructure, from dev environments, to production clusters

- [Kitematic](https://kitematic.com/) is part of the Docker tools as a simple GUI for packaged applications

- The [Docker Content Trust](http://docs.docker.com/engine/security/trust/content_trust/) and Docker Notary (both available in the [Docker Trusted Registry](https://www.docker.com/docker-trusted-registry)) allow secure and usable content distribution for developers, allowing survivable key compromise, proof of origin and protection against untrusted transports. And now, the [Yubikey](https://yubico.com/docker) can be used to sign images before pushing them to the registry. Cool!

- There is also a vulnerability scanner for containers, called Project Nautilus, that has been running in the official repos since 2 months ago, and that eventually will be released for the public as a self-service. It uses deep content analysis of the images and matches against a database of known code vulnerabilities.

- [Docker Swarm](https://docs.docker.com/swarm/) 1.0 is out. It has multi-host newtworking and persistent storage out-of-the-box, based upon the new plugin architecture for both features.

To close this session, Andrea Luzzardi ([@aluzzardi](https://twitter.com/aluzzardi)) perfomed a live demo with a cluster of 1000 dockerhosts, spinning up 50000 containers, managing swarm with `docker-compose` commands. Impressive!

The talk **The latest in Docker Engine**, by Jessie Frazelle([@frazelledazzell](https://twitter.com/frazelledazzell)) and Arnaud Porterie ([@icecrime](https://twitter.com/icecrime)) was loaded with meaty information about version 1.9:

- The new `docker network` command, explained with an example of an Elasticsearch container linked against a Kibana container, showing some stats of the Docker project. Multihost networking is out of experimental, and extensible via plugins

- Minor improvements and build-time arguments (`HTTP_PROXY`), new `ARG` Dockerfile instruction, custom STOP signal

- The new `docker volume` command, also extensible via plugins

- GID/UID remap (root is not the host root anymore!) and storage scoped by GID/UID

The also talked about the next features in the pipeline:

- Distribution rework: multiarchitecture images (powered by the so called *fat manifests*)

- Official ARM support

- Windows Server 2016 support

- IBM PowerSystems, IBMz Systems, Solaris support

- Support to [seccomp](https://code.google.com/p/seccompsandbox/wiki/overview) for syscall filtering. Jessie Frazzelle made a demo based on a [proof of concept](https://github.com/jfrazelle/callmemaybe) she wrote.

- Default to Docker Content Trust

- Splitting the tools among runtime (RunC) and builder, allowing client-side builds

- Maybe some convergence of Swarm and Engine (they have some duplicated functionality)

The talk **Docker orchestration at production scale**, by Andrea Luzzardi and Victor Vieux ([@vieux](https://twitter.com/vieux)), explained the details about the [Swarm scaling demo](https://github.com/aluzzardi/swarm-bench) of the General Session and discussed the new plugin-based networking mode that ships with two networking drivers out-of-the-box: bridge (for single host) and overlay (for multi-host) and the new plugin-based volume management that ships with local bind mount. Networking plugins (Calico, Weave, ...) and volume plugins (glusterfs, sshfs, keywhiz) are available (or almost available).

Nathan McCauley ([@nathanmccauley](https://twitter.com/nathanmccauley)) talked about **Understanding Docker Security**:

- He explained that the current model of isolation is based on namespacing (PID, mount, IPC and network), cgroups, limiting capabilities and third-party systems as SELinux and AppArmor. The work in progress in this fields is: allowing user namespaces and enabling seccomp, a granular syscal whitelist/blacklists.

- He detailed the [Docker Content Trust](https://docs.docker.com/engine/security/trust/content_trust) concept, that consists on a Notary, that translates image names to a content address, and a content addressable registry, that allows self-verifiable pulls because the name of the content is the hash of the content (something like a Merkle tree)

- About Project Nautilus, he told that it consists on a filesystem walker, with some heuristics to detect code, and a match agains a database of known vulnerable code

- Regarding, authentication and authorizations, there are pull requests for third-party systems, beyond the Unix Domain Socket and the Client Certificate, such as Kerberos, SASL, PAM, LDAP/AD, unix users...

Dave Tucker ([@dave_tucker](https://twitter.com/dave_tucker)) and [Jana Radhakrishnam](https://github.com/mrjana) gave a fantastic talk about **Docker networking deep dive**. They explained both the bridge and overlay model of networking. The latter is a very interesting one, based on VxLAN, with a centralized key-value storage (by default token-based, but swappable with Consul or other) and SERF protocol in order to exchange information among the cluster.

They also explained the roadmap of networking:

- Better IPv6 support: working by default out-of-the-box, address allocation and support for AAAA records

- DNS-based service discovery

- Encryption of the overlay network 

- Official *proxy* container for tying networks together

- *Offline* networks

And that's all for the first day!
