+++
subtitle = ""
title = "FOSDEM 2017"
bigimg = ""
date ="2017-02-07T19:41:00+01:00"

+++

Last weekend I went to [FOSDEM 2017](https://fosdem.org/2017/). In this post, I will share links and notes regarding several talks I have attended to.

<!-- TEASER_END -->

Etiene Dalcol ([@etiene_d](http://www.twitter.com/etiene_d)) spoke about [Making wearables with NodeMCU](http://video.fosdem.org/2017/K.3.201/nodemcu_wearables.mp4). Although I have very little knowledge of the [Lua](http://www.lua.org/) programming language I attended to this talk because I had heard about [NodeMCU](http://www.nodemcu.com/index_en.html) as a firmware to power the mighty [ESP8266](https://en.wikipedia.org/wiki/ESP8266) dev kits. During the talk, Etiene discussed the different boards available in the market (I didn't know about the ESP8285, a good alternative), how to upload the Lua code to them and several examples, including a party dress with LEDs attached that changed patterns controlled by her wristwatch (and even by the music volume itself).

Daniel Stenberg ([@badger](http://www.twitter.com/badger)) the creator of the omnipresent tool [cURL](https://curl.haxx.se/), gave a talk named [You know what's cool? Running on billions of devices](http://video.fosdem.org/2017/Janson/curl.mp4). He explained the origins of the tools, its history (20 years at an average of 2 hours a day of his time!), and many anecdotes, as when someone e-mailed him to fix his car Satnav because he found his e-mail address in the terms & conditions of the GPS application (in the `libcurl` license). It's amazing how such a tool, that seems quite simple, can demand so much development (and bug-fixing) time. And how fragile our ecosystem is when there is a shared dependency among our smartphones, navigation systems and even music players.

[Brooks Davis](https://www.linkedin.com/in/brooks-davis-a2105b20) dissected a variation of the C language's *Hello World!* in [Everything you ever wanted to know about "hello world"](https://fosdem.org/2017/schedule/event/hello_world/attachments/audio/1851/export/events/attachments/hello_world/audio/1851/20170204_fosdem_helloworld.pdf). Basically, he explained step by step (and with descriptive [flame graphs](http://www.brendangregg.com/flamegraphs.html)) the syscall timeline and the operations involved in a very simple program. Sometimes a bit *hard to chew*, but certainly very instructive and deeply technical.

Roberto Colistete Jr ([@rcolistete](http://www.twitter.com/rcolistete)) talked about *Scientific MicroPython for Microcontrollers and IoT*. I knew Micropython before (I own the first [WiPy](https://www.pycom.io/product/wipy/) and the [BBC:Microbit](https://www.microbit.co.uk/)) but I didn't know that there was already a firmware for the ESP8266 (yeah!). I even saw it in action in the hands of Roberto, taking measurements of distance. Roberto explained the importance of filtering sensor data as any other error-prone measurement, because sometimes it's easy to skip that phase and accept spurious values.


JJ Merelo ([@jjmerelo](http://www.twitter.com/jjmerelo)) gave a lightning talk about [Learning Programming in the XXI century](http://video.fosdem.org/2017/H.2215/goodbye_hello_world.mp4). Quite a motivational talk, he covered interesting subjects as learning polygloth programming, with a Git repo (or *Git before open*, in his own words) and with cloud by default.

On the Go devroom, Michael Schubert ([@schux00](http://www.twitter.com/schux00)) explained how to use [eBPF](https://video.fosdem.org/2017/H.1302/go_bpf.mp4) from Go, with the library [gobpf](https://github.com/iovisor/gobpf). Although very powerful, eBPF is still hard to use even with this library, as you have to write C code for the maps. I have taken note to invest some time in the future in order to learn more about these tracing tools that are now available in the Linux Kernel.

The most funny and entertaining talk (on my view) has been [High-performance IoT using Go and Gobot](https://video.fosdem.org/2017/H.1302/go_gobot.mp4). Ron Evans ([@deadprogram](http://www.twitter.com/deadprogram)), creator of the [gobot](http://gobot.io/) framework, did quite a lot of live demos controlling Arduinos, LEDs, sensors and even a flying drone hovering around the room. This framework is amazingly designed, with some very good abstractions for the devices, drivers and adapters, and is leveraged by the concurrency of Golang in order to maintain several loops for controlling actions of a swarm of robots. I really recommend you to watch the video!

Santiago M. Mola ([@mola_io](http://www.twitter.com/mola_io)) presented [go-git: A pure implementation of Git](https://fosdem.org/2017/schedule/event/go_git/): a library developed by the [Source{d}](http://www.sourced.tech/) engineering team, to interact with Git, in a pure-Go (no C bindings) way. I have also learnt about another project that they have developed ([go-stable](https://github.com/mcuadros/go-stable)) that provides versioned URLs for Go packages. I will use it in my next projects.

Frank Wessels, from [Minio](https://minio.io/), showed us [High performance and scaling techniques in Golang using Go Assembly](https://fosdem.org/2017/schedule/event/go_scaling/). Basically, he explained how they did implement the Blake2 and SHA-256 using Plan9/Golang assembly, leveraging the capabilities of the ARM processor and achieving speed ups up to 100x (wow!).

Francesc Campoy ([@francesc](http://www.twitter.com/francesc)) gave his classic yearly talk of [The State of Go](http://video.fosdem.org/2017/H.1302/go_state.mp4). The news about the upcoming 1.8 release, including the language changes (only one) and the improvements in the tools (many).

In the lightning talks, Ernesto Jiménez ([@ernesto_jimenez](http://www.twitter.com/ernesto_jimenez)) gave some good advice on how to make [backwards compatible code](https://speakerdeck.com/ernesto_jimenez/keep-packages-backwards-compatible) (being careful on the exposed elements of your packages) and Tobias ([@dh1tw](http://www.twitter.com/dh1tw)) explained the [remoteAudio](https://github.com/Dh1tw/remoteAudio) system based on Golang developed for remotely controlling the spanish Ham Radio station ED1R.

Finally, in the containers devroom, Jorge Salamero ([@bencerillo](http://www.twitter.com/bencerillo)) used [Sysdig](https://sysdig.com/) to troubleshoot step-by-step an [incident in a Kubernetes Cluster](https://sysdig.com/blog/sysdigkubernetes-adventure-part-1-kubernetes-services-work/).
