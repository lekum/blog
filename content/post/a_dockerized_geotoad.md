+++
subtitle = ""
title = "A Dockerized GeoToad for paperless Geocaching"
bigimg = ""
date ="2015-08-15T22:41:17+02:00"

+++

In the recent years the game of [Geocaching](https://www.geocaching.com/) has steadily grown in popularity as a hobby and outdoor activity. Basically, Geocaching consists on a worldwide-played game in which people hide *caches* (containers with at least a paper log) and publish their coordinates over the internet so that other players can find them and log them. It is very addictive, believe me.

<!-- TEASER_END -->

There are many applications to play Geocaching with a smartphone, being probably the most famous [c:geo](http://www.cgeo.org/). If you have a data connection available, it works great, but when you are in a remote area or abroad (and you don't want to sell your house to pay the roaming taxes), an offline strategy is more suitable. C:geo and download caches for later offline usage, but doing so for many caches is tedious. C:geo, as well as many other applications and devices (GPS receivers, for example), is able to load a [GPX](https://en.wikipedia.org/wiki/GPS_Exchange_Format) file with the information of the caches, including the coordinates, description, hints, logs written by the latest visitors and other bites of useful information.

There is a great piece of software called [GeoToad](https://github.com/HughP/geotoad) that allows to create GPX files given certain criteria, as caches some miles around certain coordinates, of a given size, or with some attributes. **Warning**: use this tool at your own risk. *Geocaching.com* could ban you from abusing their terms of service, as it is a web scraping tool.

Playing with Docker, I have found interesting to create a Dockerfile for an image with GeoToad and its dependencies installed:

```Dockerfile
FROM ubuntu:14.04
MAINTAINER Alejandro Guirao <lekumberri@gmail.com>

RUN apt-get -qqy update
RUN apt-get -qqy install ruby git

RUN mkdir -p /opt/geotoad
RUN mkdir -p /opt/gpx
RUN git clone https://github.com/HughP/geotoad.git /opt/geotoad

ENTRYPOINT [ "/opt/geotoad/geotoad.rb" ]
CMD []
```

Basically we are installing the dependencies (`ruby`, `git`), cloning the GeoToad repo and marking as entrypoint the `geotoad.rb` script. In addition, we are creating a folder (`/opt/gpx`) in which we can, for example, save the GPX files. The Dockerfile is published on this [repo](https://github.com/lekum/geotoad-docker), so feel free to contribute with pull requests if you want so.

So, being in the same folder as the Dockerfile, we can build an image (e.g. `lekum/geotoad`, use your own registry and name) with this command:

```
docker build -t lekum/geotoad .
```

Once built, we can run a container with it:

```
docker run -ti -v $PWD:/opt/gpx --name gt01 lekum/geotoad
```

This command runs the docker container named `gt01` based on the image `lekum/geotoad` in an interactive terminal, mapping our current working directory with the `/opt/gpx` folder inside the container.

After hitting `ENTER`, we will be presented with the Text User Interface of Geotoad:

```
==============================================================================
:::              // GeoToad (CURRENT) Text User Interface //               :::
==============================================================================
(1)  GC.com login [REQUIRED         ] | (2)  search type          [location  ]
(3)  location     [REQUIRED         ] | (4)  distance maximum (mi)  [      10]
- - - - - - - - - - - - - - - - - - - + - - - - - - - - - - - - - - - - - - -
(5)  difficulty           [1.0 - 5.0] | (6)  terrain               [1.0 - 5.0]
(7)  fav factor           [0.0 - 5.0] | (8)  cache size            [any - any]
(9)  cache type   [                                                       any]
(10) caches not found by me       [ ] | (11) caches not found by anyone    [ ]
(12) cache age (days)     [  0 - any] | (13) last found (days ago) [  0 - any]
(14) title keyword       [          ] | (15) descript. keyword [             ]
(16) cache not found by  [          ] | (17) cache owner isn't [             ]
(18) cache found by      [          ] | (19) cache owner is    [             ]
(20) EasyName WP length         [  0] | (21) include disabled caches       [ ]
(22) caches with trackables only  [ ] | (23) no Premium Member Only caches [ ]
- - - - - - - - - - - - - - - - - - - + - - - - - - - - - - - - - - - - - - -
(24) output format  [gpx            ] | (25) filename   [automatic           ]
(26) output directory    [/root                                              ]
==============================================================================
** Verbose (debug) mode disabled, (v) to change
-- Enter menu number, (s) to start, (R) to reset, or (Q) to exit --> 
```

You will have to provide, at least, a username and password of Geocaching, and a location (coordinates, for example) to start searching. In addition, we would like to change the output directory to `/opt/gpx`. Once we are done with the settings, entering `s` will start the web scraping process. In one the first lines of the log, we will see the equivalent command-line invocation of `geotoad`. For example, this could be the command line for a query of *traditional* caches 5 km around the Google's geolocation of Barcelona:

```
geotoad.rb --cacheType=traditional --delimiter='|' --distanceMax=5 --output='/opt/gpx/barcelona.gpx' --queryType=location Barcelona
```

If we don't wan't to wait watching the output of the geotoad process, we can detach from the container using the key combination `Ctrl-P` + `Ctrl-Q`. When the process finishes, we will have the `barcelona.gpx` file in our current working directory. Yikes!

If we want to run it quietly, we can lauch the container overriding the empty `CMD` of the image with the options of the geotoad command, starting the container in the background. In this case, in addition, we will need to supply the username and password via command-line switches:

```
docker run -d -v $PWD:/opt/gpx --name gt02 lekum/geotoad  -u geocaching_username -p geocaching_password --cacheType=traditional --delimiter='|' --distanceMax=5 --output='/opt/gpx/barcelona.gpx' --queryType=location Barcelona
```

This way we can run some queries in parallel, or programatically, or distributing the load among Docker hosts. We can upgrade GeoToad to new versions just building a new image. We are moving from using a script in a terminal to generating GPX as an output of a pretty standardized tool, a container. Implementing the generation of GPX files as a cloud service is now almost trivial.

GeoToad is not a very complex tool and thus it does not truly benefit from the isolation that the containers provide, but I wanted to show with this simple example how containerizing apps leads to new workflows and dynamics otherwise very difficult to implement on traditional environments.
