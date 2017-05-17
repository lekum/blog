+++
subtitle = ""
title = "Lighter Python images using multi-stage Dockerfiles"
bigimg = ""
date = "2017-05-17T14:55:27+02:00"

+++

A very interesting feature coming with Docker 17.05 is [multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build) in the Dockerfile. This feature allows to implement a simple *pipeline* during the build phase and carry artifacts between stages. Previously, if you wanted to deliver slim images, you would have to implement the *builder* pattern.

First, you would build an image with all the development dependencies, build the project and generate your artifacts, export them using `docker cp` or anything similar and then add those files to a new image, with a slimmer base. This was a common patter, for example, for generating lightweight images of statically-compiled Golang applications.

With the multi-stage builds, it is possible to specify this workflow inside the Dockerfile. Let's assume that we want a Python image with the [pycrypto](https://pypi.python.org/pypi/pycrypto) package installed, as described in the file `requirements.txt`:

```
$ cat requirements.txt

pycrypto
```

We could use this Dockerfile:

```
FROM python:3 as python-base
COPY requirements.txt .
RUN pip install -r requirements.txt

FROM python:3-alpine
COPY --from=python-base /root/.cache /root/.cache
COPY --from=python-base requirements.txt .
RUN pip install -r requirements.txt && rm -rf /root/.cache
```

There are a few points to note here:

- The syntax of the Dockerfile allows several `FROM` directives, one per image/stage, that can be tagged using the `as` keyword.
- The `COPY` directive accepts a `--from=` option to point to the artifacts and files from a previous image/stage.
- I have chosen `pycrypto` as it is a library that require compilation at installation time. Thus, we are using a *fat* image named `python:3` that ships with GCC and libraries.
- The Python [wheels](http://pythonwheels.com/) that are downloaded or generated as a result of the compilation are stored in the `~/.cache/pip/wheels` folder, so passing this folder as an artifact between stages prevents a succesive download and compilation of the packages, allowing us to use a slimmer image (`python:3-alpine`).

The resulting image has a reduced surface attack, less dependencies installed and it is definitely slimmer. The `python:3` image with `pycrypto` installed weighs `697 MB`. The alpine-based, only `96.4 MB`.

