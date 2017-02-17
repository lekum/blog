+++
subtitle = ""
title = "FOSDEM 2016 Day Two"
bigimg = ""
date ="2016-02-06T19:01:30+01:00"

+++

[]()

On Sunday, I attended to some talks in the [Configuration Management devroom](https://fosdem.org/2016/schedule/track/configuration_management/). These are the notes and links from the talks.

<!-- TEASER_END -->

Arthur Lutz ([@arthurlutz](https://twitter.com/arthurlutz)) talked about [After describing your infrastructure as code, reuse that to monitor it](http://slides.logilab.fr/2016/fosdem_describe_it_monitor_it/#/after-describing-your-infrastructure-as-code-reuse-that-to-monitor-it). He presented an architecture in which he ran the [Munin](http://munin-monitoring.org/) monitoring directly from [Salt](http://saltstack.com/) minions. It was interesting to see the variety of [metrics](http://slides.logilab.fr/2016/fosdem_describe_it_monitor_it/#/what-we-graph-monitor) they are currently watching.

Marco Ceppi ([@marcoceppi](https://twitter.com/marcoceppi)) gave a talk entitled *Beyond config management: Tackling orchestration and modelling on top of config management*. Basically, he described the evolution of Configuration Management and how he considers that *modelling* is the next step: using abstract primitives to become platform and tool agnostic. It is basically the idea behing Canonical's [Juju](http://www.ubuntu.com/cloud/juju), a project in which Marco is involved. He also mentioned a similar tool recently released by Wallmart labs called [OneOps](http://www.walmartlabs.com/2016/01/oneops-now-available/). I will give both of them a try!

Walter Heck ([@walterheck](https://twitter.com/walterheck)) explained the learnt lessons of a very big project of Puppet implementation in a manually-managed-infrastructure enterprise in *War Story: Puppet in a Traditional Enterprise*. Interesting to see that many problems were not tech-related but human-friction related. Changing the way we work is hard but is absolutely necessary for a sysadmin.

Eric Sorenson ([@ahpook](https://twitter.com/ahpook)) discussed about *Flexibility and Power in Puppet 4 Language*. The most important features of the new version of the tool are: improved network communication protocol (backwards compatible), enhanced packaging and changes in the language (types, support for `each` Ruby construct, ability to define functions, hierarchy improvements).

Peter Souter ([@petersouter](https://twitter.com/petersouter)) performed a brilliant talk  about [Hardening your Config Management](http://www.slideshare.net/petems/hardening-your-config-management-security-and-attack-vectors-in-config-management). On my opinion, the best talk among the ones that I have attended to in this edition of FOSDEM. It was fully packed of wise advice such as:

- Follow the [OWASP security principles](http://www.owasp.org/index.php/Secure_Coding_Principles#Security_principles)
- Remove data from code using data abstraction layers. For example: [hieras](https://docs.puppetlabs.com/hiera/1/) in Puppet, [data bags](https://docs.chef.io/data_bags.html)/[attributes](https://docs.chef.io/attributes.html) in Chef, [roles](http://docs.ansible.com/ansible/playbooks_roles.html) in Ansible, [grains](https://docs.saltstack.com/en/latest/topics/targeting/grains.html)/[pillars](https://docs.saltstack.com/en/latest/topics/pillar/) in Salt
- Encrypt data with your application tooling: [hiera-yaml](https://docs.puppetlabs.com/hiera/3.0/configuring.html) in Puppet, [chef-vault](https://github.com/chef/chef-vault) in Chef, [ansible vault](http://docs.ansible.com/ansible/playbooks_vault.html) in Ansible, [salt.modules.gpg](https://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.gpg.html) in Salt, [cf-keycript](https://github.com/cfengineers-net/cf-keycrypt) in CFEngine
- Use, if possible, external secret servers, like OpenStack's [Barbican](https://wiki.openstack.org/wiki/Barbican), CloudFlare's [Red October](https://github.com/cloudflare/redoctober) or Hashicorp's [Vault](https://www.hashicorp.com/blog/vault.html)
- Use [git-cript](https://www.agwa.name/projects/git-crypt/) for achieving a transparent file encryption in git
- Use automated (spec testing, linting) and manual code reviews
- Compare your security with others when possible
- Establish *game days* trying to break your security and seeing how much damage is caused and how long would it take to notice the intrusion
- Get a baseline of the status of the infrastructure and monitor it for unexpected changes (using [riemann](http://riemann.io/), [statsd](https://github.com/etsy/statsd), [ELK](https://www.elastic.co/products/elasticsearch), [collectd](https://collectd.org/)...)
- Search for suspicious activity in your logs, as 4xx, 5xx errors when there is no activity, unexplained increases in temperature of the machines...
- Use your tooling sensitive-protection on (like `no_log: True` in Ansible)
- Use automatic hardening frameworks as [hardening.io](http://hardening.io/) (seems incredibly useful!)
- Follow SSH hardening standars (whitelisted access, bastion hosts, restrict users, increase key strength, rotate keys, use pre-populated knownhosts)

[Mark Hoffmann](http://h-rd.org/) spoke about [Literate DevOps](http://h-rd.org/wp-content/uploads/2016/01/litdevops-fosdem2016.pdf). He presented this concept, based upon the [Literate Programming](https://en.wikipedia.org/wiki/Literate_programming) principles, as *documentation with embedded executable DevOps code*. He presented an implementation with [Emacs](https://www.gnu.org/software/emacs/) and [Org mode](http://orgmode.org/). Good food for thought that reminds me the [Jupyter notebook](http://jupyter.org/) approach.

[Stéphan Gorget](http://phantez.net/), from Facebook engineering, gave a talk about *Managing a complex DNS environment*. He explained the really complex architecture that they have deployed in order to attain reliable DNS updates in every machine. They use Unbound and TinyDNS in each host, with a pipeline that merges the manual changes and automated changes, distributes the config files using BitTorrent and restarts the daemons using a framework named [sparts](https://pypi.python.org/pypi/sparts). For the external DNS, they use a system that performs TLS termination near the users. They distribute a unique name to look up to a small percentage of users, allowing Facebook to buld a map of the users and resolvers. This map is updated every 2 minutes and sent to the DNS servers, in order to minimize latencies.

The last talk of the devroom was [Config Management and Containers](https://fosdem.org/2016/schedule/event/config_management_containers/attachments/slides/1110/export/events/attachments/config_management_containers/slides/1110/lazypower_fosdem2016_configmanagement_and_containers.pdf) by Charles Butler ([@lazypower](https://twitter.com/lazypower)). H explained how containers are disrupting the configuration management ecosystem and the Juju's approach to modelling them: by using the [Charm Layer for Docker](https://github.com/juju-solutions/layer-docker). A nice tool, that even supports `docker-compose.yml` files.

And I finally attended to the closing keynote [Putting 8 Million People on the Map](http://mirror.onet.pl/pub/mirrors/video.fosdem.org/2016/janson/putting-8-million-people-on-the-map.mp4), by Blake Girardot ([@BlakeGirardot](https://twitter.com/BlakeGirardot)), from the [Humanitarian OpenStreetMap Team](https://hotosm.org/). It is amazing how the work of volunteers mapping all over the world can help in case of disasters, and the [OSM Tasking Manager](http://tasks.hotosm.org/) is a blast. It is very easy to [get involved](https://hotosm.org/get-involved/disaster-mapping). Happy hacking!
