+++
subtitle = ""
title = "Bringing SSL/TLS and HTTP/2 with Caddy"
bigimg = ""
date ="2016-04-08T08:58:48+02:00"

+++

Some weeks ago, [Fernando](https://twitter.com/fern4lvarez) gave a very interesting talk about [Caddy](https://caddyserver.com/) and [HTTP/2](https://http2.github.io/) at the [Madrid Golang users group meetup](http://www.meetup.com/es-ES/go-mad/events/228944863/). It immediately caught my attention and decided to get hands on with it.
<!-- TEASER_END -->

Basically, Caddy is a web server that implements the HTTP/2 protocol for the modern web. This version provides many advantages over HTTP/1.1 as multiplexing and [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) by default. For me, the selling points of Caddy are two:

- It is very straightforward to configure. Sometimes, the config file is just a one-liner.
- It has first-class and out-of-the-box support for the [letsencrypt](https://letsencrypt.org/) service. Basically, you'll have free SSL/TLS Certificates for your sites that are automatically renewed by Caddy. Amazing!

I encourage you to browse the [slides](https://github.com/fern4lvarez/presentations/raw/master/goMad2016_CaddyLetsEncryptHTTP2.pdf) and read the [Getting Started](https://caddyserver.com/docs/getting-started) section of the Caddy documentation. It is really simple to use.

I decided to give it a try so I skimmed through the documentation, and realised that I just had to [download](https://caddyserver.com/download) the binary for my system and create a Caddyfile like this:

```caddyfile
lekum.org {
    root /path/to/my/static/files
    log access.log
}
```

and start the binary using a systemd unit service, something like this:

```ini
[Unit]
Description=Caddy web server
After=network.target

[Service]
User=my_user
WorkingDirectory=/path/to/the/caddyfile/folder
ExecStart=/path/to/bin/caddy -agree -email my_email
LimitNOFILE=8192

[Install]
WantedBy=multi-user.target
```

Make sure that the binary has permissions to bind ports 80 and 443. For example, issuing the command:

```bash
setcap cap_net_bind_service=+ep ./caddy
```

Caddy retrieves a SSL/TLS certificate for your site (it is important that you configure correctly the FQDN in the Caddyfile because it performs a challenge/response in order to validate your server), stores it in your server and launches a [Goroutine](https://tour.golang.org/concurrency/1) that seamlessly renew the certificate before the 90-day lifetime of the letsencrypt certificate expires.

Happy hacking!
