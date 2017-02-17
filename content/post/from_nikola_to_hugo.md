+++
subtitle = ""
title = "From Nikola to Hugo"
bigimg = ""
date = "2017-02-17T20:00:24+02:00"

+++

Today I have migrated the static generator for this site from [Nikola](https://getnikola.com/) to [Hugo](https://gohugo.io/). Part of the motivation has been to try a new technology, part of it, having less movable parts. Nikola is great but carries quite a lot of dependencies (specially with the themes). My previous workardound involved building a [Docker image]( wa://hub.docker.com/r/lekum/docker-nikola/) with the tools, and using that image inside a [GitlabCI](https://about.gitlab.com/gitlab-ci/) pipeline that built the statics and deployed them to my server.

My articles were written in [Markdown](https://en.wikipedia.org/wiki/Markdown), so I didn't have to change the contents. I made a *quick-and-dirty* Python script to reformat the headers of each article (specially `title` and `date`). Appart from that, I chose a [theme](https://github.com/halogenica/beautifulhugo) and wrote a small [config.toml](https://github.com/lekum/blog/blob/master/config.toml).

Hugo is a [Golang](https://golang.org/)-compiled single binary, so it is operationally much simpler to deploy, on my laptop and on the CI pipeline. For this new version of the blog, I have moved the repo to [Github](https://github.com/lekum/blog) and the CI to [Travis](https://travis-ci.org/lekum/blog). This is my new `.travis.yml`:

```
language: go
go:
  - 1.8
before_install:
  - openssl aes-256-cbc -K $encrypted_84f17b3aef8a_key -iv $encrypted_84f17b3aef8a_iv -in deploy/server.pem.enc -out deploy/server.pem -d
addons:
  apt:
    packages:
      - rsync
      - openssh-client
install:
  - eval $(ssh-agent -s)
  - chmod 600 deploy/server.pem && ssh-add deploy/server.pem
  - mkdir -p ~/.ssh
  - echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config
  - GOPATH=$HOME/go go get github.com/spf13/hugo
script:
  - "$HOME/go/bin/hugo"
after_success:
  - rsync -avz --delete public/ $BLOG_DEPLOYMENT_URL
```

The only *tricky* part is allowing ssh access to my server. After reading the [documentation](https://docs.travis-ci.com/user/encrypting-files/), I have opted for commiting an encrypted version of a `.pem` file that I have generated for this purpose. The encryption is performed once using the `travis` command line interface:

```
travis login
travis encrypt-file deploy/server.pem --add
```

This command executes an AES-256 simmetric encryption and add the line to your `.travis.yml` to perform the decoding at the beginning of the execution.

The only issue that I currently have is that the old URLs to the posts do not work, because Hugo expects the posts inside a `post` folder. I definitely need to read the documentation :-)
