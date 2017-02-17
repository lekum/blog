+++
subtitle = ""
title = "Dockercon EU 2015 Day Two"
bigimg = ""
date ="2015-11-19T08:50:20+01:00"

+++

The **General session** of day two was focused on the *solutions* layer of the Docker ecosystem:

- [Mariana Tessel](https://www.linkedin.com/in/mariannatessel) explained the automated builds 2.0, that allow per-repo dedicated builders and dynamic matching (regex for branches and tags), and presented Tutum as the official CaaS (*container-as-a-service*) provided by Docker and fully integrated with `docker-compose`

- Scott Johnston ([@scottcjohnston](https://twitter.com/scottcjohnston)) and Nathan McCauley talked about the Docker Trusted Registry and the [Docker Universal Control Plane](https://www.docker.com/universal-control-plane). The latter is promising tool with an appealing web UI focused on deploying containers over different providers (AWS, Azure, DigitalOcean, SoftLayer) using the same Docker API. This tool also ships with secrets management (based on labels), monitoring, integration with LDAP/AD and auditing. You can join the beta at [docker.com/try-ducp](https://www.docker.com/try-ducp)

<!-- TEASER_END -->

John Fiedler ([@johnfiedler](https://twitter.com/johnfiedler)) spoke about **How to be succesful running docker in production**. It was an enlightening talk distilling wisdom because he has been running Docker in production since 2013 (version 0.5) so he has gathered a big amount of experience on the field:

- He explained that not every service is a good fit to be containerized. Web servers, API servers are great, but databases and CI/CD are not and will cause nightmares

- Don't try to implement a PaaS if you are starting out, start with a dockerized test environment and move to production

- Old tools (like Chef) are still useful in a world of immutable infrastructure, you only have to know what to do with them

- Secure your containers with tools as [Twistlock](https://www.twistlock.com/), [Conjur](https://www.conjur.net/solutions/docker) or [Banyanops](http://www.banyanops.com/)

- Use a *polished* registry (such as [quay.io](https://quay.io/))

- Stick to AUFS in Ubuntu and Device Mapper in Amazon Linux (personal recommendation based on the headaches he has had to suffer)

- Keep a low *container-to-host* ratio in order to minimize isolation and scheduling problems

- Use a sidekick container to orchestrate the containers in a host. For example, AWS has [one](https://github.com/aws/amazon-ecs-agent) for ECS.

Michael Neale ([@michaelneale](https://twitter.com/michaelneale)) spoke about **Persistent, stateful services with Docker clusters, namespaces and docker volume magic**. Basically he developed a sidekick/supercontainer that offered volume management to the client containers:

- He periodically (about each 5 min.) does a backup of a LVM snapshot of the data partitions attached to the containers and sends them to S3

- When the client container requests a volume, the supercontainer fetches it from S3, mounts it on the host (because the supercontainer runs in privileged mode with the host filesystem mounted) and finally enters the namespace of the container and attaches it to the container

Michael said that nowadays he would do it in a different way, using the Docker volume API. He mentioned some tools to manage stateful volumes:

- [REX-Ray](https://github.com/emccode/rexray)
- [Flocker](https://clusterhq.com/flocker/introduction/)
- Kubernetes volumes support
- Tutum volumes support

Adrien Blind ([@adrienblind](https://twitter.com/adrienblind)) and Laurent Grangeau ([@laurentgrangeau](https://twitter.com/laurentgrangeau)) talked about **The missing piece when docker networking and services finally unleashes software architecture 2.0**. I have learned how to use the experimental support for [networking](https://docs.docker.com/compose/networking/) in `docker-compose`.

Finally, the **Gran final** was a general session in which some projects of the [Docker Global Hack Day](http://www.docker.com/community/docker-global-hack-day-3) were presented:

- A [proof of concept](http://blog.mantika.io/GHD) by the Mantika Team of container live migration
- A very promising and motivating hack to support [unikernels](http://unikernel.org/blog/2015/unikernels-meet-docker/) management via the Docker interface
- [Dockercraft](https://github.com/docker/dockercraft), a Minecraft client to visualize and manage Docker containers. Very cool live demo!

My conclusions after the Dockercon are:

- The conference organization has been almost perfect, everything was carefully planned and ran smoothly. Kudos!
- The talks by the Docker team about roadmap were probably the most valuable ones
- The talks about personal experiences and hacks with Docker are fun, but usually not very profitable as the ecosystem is so wide that *your mileage may vary*

I hope that these shared notes may be useful for someone. Thanks for reading!
