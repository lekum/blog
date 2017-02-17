+++
subtitle = ""
title = "dotGo 2016"
bigimg = ""
date ="2016-10-14T11:01:40+02:00"

+++

This week I have attended to the [dotGo2016](http://www.dotgo.eu/) conference in Paris. It is a one-day, one-track, 20-minutes-per-talk conference. Very few laptops, people were really focused attending to the dense talks. Here I will highlight some concepts that I have found interesting in some of the talks:

<!-- TEASER_END -->

Dave Cheney([@davecheney](https://twitter.com/davecheney)) talked about first class functions (*Don't fear the first class functions*), how to implement them in Golang and why they are sometimes a good choice because of the advantages of passing behaviour (encoded in functions) over raw data structures. You can find the example code [here](https://github.com/davecheney/presentations/tree/master/do-not-fear-first-class-functions). Some examples are inspired by Bryan Boreham's [An Actor Model in Go](https://www.youtube.com/watch?v=yCbon_9yGVs) talk from  Golang UK 2016 conference.

Damian Gryski([@dgryski](https://twitter.com/dgryski)) discussed about bottlenecks in memory access time, showed some performance metering examples with `kcachegrind` and `perf` and stated the importance of trying to code in a way that processor cache usage is maximized. I have recently heard about a similar concept ([Mechanical sympathy](https://changelog.com/gotime/6)) and I find it quite appealing.

Dmitri Shuralyov ([@shurcool](https://twitter.com/shurcool)), contributor to [GopherJS](https://github.com/gopherjs/gopherjs), discussed about the origin and motivation of the project, the evolution of it and the pending features.

Péter Szilágyi ([@peter_szilagyi](https://twitter.com/peter_szilagyi)), core developer at the [Ethereum](https://ethereum.org/) blockchain-powered platform, talked about how [Immutability in Go](https://ethereum.karalabe.com/talks/2016-dotgo.html) helped them cope with the attacks to their platform.

Lucas Clemente ([@luke_r2d2](https://twitter.com/luke_r2d2)) presented the [QUIC](https://en.wikipedia.org/wiki/QUIC) protocol and his package [quic-go](https://github.com/lucas-clemente/quic-go) to use it. It is great that the library presents an interface really similar to `net/http`. And nice to learn that [Caddy](https://caddyserver.com/) can run with a `-quic` flag!

Jaana Burcu Dogan ([@rakyll](https://twitter.com/rakyll)) gave a lightning talk about [Codegen Inspection](http://go-talks.appspot.com/github.com/rakyll/talks/gcinspect/talk.slide#1). I recommend you to check the slides and find out how to disassemble your binaries and examine them.

Elias Naur ([@elias_naur](https://twitter.com/elias_naur)), contributor to [Go Mobile](https://github.com/golang/mobile), showed the power of the integration between Golang and Java/.Net (in both directions) for mobile apps development.

Kelsey Hightower ([@kelseyhightower](https://twitter.com/kelseyhightower)) presented [kargo](https://github.com/kelseyhightower/kargo), a proof-of-concept library he has developed to allow golang binaries to self-deploy to kubernetes without any additional files or commands. Entertaining as always, with a vibrant demo, although I do not see a practical application, I follow his reasoning on the possible use case of self-deploying applications.

Javier Fernández Provecho ([@javierprovecho](https://twitter.com/javierprovecho)) gave a lightning talk about skimming your binaries and shaving some extra megs with `ldflags` and [UPX](https://upx.github.io/) tools.

Rhys Hiltner([@rhyshiltner](https://twitter.com/rhyshiltner)) showed some performance metering tricks with [pprof](https://golang.org/pkg/net/http/pprof/) and the `go tool trace`

Katrina Owen ([@kytrinyx](https://twitter.com/kytrinyx)) explained both how do they injected testing in a project when debugging an issue and also the flocking pattern for refactoring (although she managed not to use the word *refactoring*), based on finding similarly-shaped patterns and iterating the abstraction until an acceptable result is found, rather than doing an upfront redesign

Brad Rydzewski ([@radrydzewski](https://twitter.com/bradrydzewski)), creator of [Drone](https://github.com/drone/drone) CI, explained several patters of pluging integrations in Golang:

- Embedded plugins (such as in Caddy web server)
- Net Plugins (such as in Hashicorp, Docker... products), via RPC
- RPC plugins using streams (powered by `os/exec` and `stdin` and `stdout` redirection)
- Net Plugins + docker run: a way to achieve maximum isolation, at the cost of worst performance. This is the model for the Drone CI plugin system.

Matthew Holt ([@mholt6](https://twitter.com/mholt6)) explained how [ACME](https://en.wikipedia.org/wiki/Automated_Certificate_Management_Environment) protocol works and why it is so important and such a game-changer for certificate management.

Finally, [Robert Griesemer](https://github.com/griesemer) gave a master talk on how to prototype using Golang, following the [example](https://github.com/griesemer/dotGo2016) of a new syntax allowing multi-dimensional index operators, powered by a transformation of the AST using a toolchain with `go/parser` and `go/printer`. Great!

Bonus: Florine Pigny live-sketched the conference [here](https://www.dotconferences.com/blog/dotgo-2016-live-sketching), through these pictures you can get a sense of the place and the atmosphere.
