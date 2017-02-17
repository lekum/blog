+++
subtitle = ""
title = "Caddy 0.9 has arrived"
bigimg = ""
date ="2016-07-19T23:00:12+02:00"

+++

Yesterday, Matt Holt announced the [0.9 Release of Caddy server](https://caddyserver.com/blog/caddy-0_9-released). It is not a normal release, there are very important changes in the project. Let's see some of them.

<!-- TEASER_END -->

- Caddy has been redesigned into a core + plugins architecture. Now, the server part is a plugin, so you can basically exchange HTTP for DNS, mail, SSH, Git... **yes, it is amazing, that means TLS out-of-the-box for any supported protocol server**. Wow, this is a game-changer.

- You can use ACME [DNS Challenge](https://caddyserver.com/docs/automatic-https#dns-challenge) to obtain the letsencrypt certificates, in case you cannot bind ports 443 and 80 to do it.

- You can use wildcards and paths for the site addresses.

- The `proxy` directive has been improved and there is a finer control of headers.

If you have some minutes, take a look on the [blog post](https://caddyserver.com/blog/caddy-0_9-released) and the [Changelog](https://github.com/mholt/caddy/releases/tag/v0.9.0) because this version is an amazing (re)engineering exercise.
