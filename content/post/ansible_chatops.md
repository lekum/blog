+++
subtitle = ""
title = "Implementing an Ansible ChatOps bot"
bigimg = ""
date ="2016-01-21T11:20:39+01:00"

+++

After watching the talk [ChatOps at GiHub](https://www.youtube.com/watch?v=NST3u-GjjFw) I decided to make a simple ChatOps bot to be able to issue one-liner Ansible commands from a Slack channel. Basically, I didn't want to open the VPN for making quick-and-dirty operations :-)
<!-- TEASER_END -->

So I decided to try [Errbot](http://errbot.io/), a Pythonic library for creating chatbots that supports many backends out-of-the-box, including Slack. The library is very easy to use, you just have to install it, make a [config.py](https://raw.githubusercontent.com/errbotio/errbot/master/errbot/config-template.py) file with the settings of your backend, and develop a plugin to program the behaviour of the bot.

As I only wanted the functionality to issue `ansible` commands, [the plugin that I developed](https://github.com/lekum/ansiblebot) was really simple:

```
from errbot import BotPlugin, botcmd
from subprocess import check_output

class AnsibleBot(BotPlugin):
    """Bot that executes Ansible commands remotely"""

    @botcmd
    def ansible(self, msg, args):
        """Run one-time Ansible commands"""
        cmd = " ".join(("venv/bin/ansible", args))
        prefix = "".join(("Executing `ansible ", args, "`:"))
        return "\n".join((prefix, check_output(cmd, shell=True)))
```

Basically, the bot watches the channel for messages starting with `!ansible` and then launches the ansible command with the rest of the message. Probably, it would be a better idea to implement a more robust parser and not just allow arbitrary code execution. The current approach is just a fast hack, and it poses certain security risks, so please use this code under your own responsibility :-)

In order to use it from Slack, we need to enable the custom integration of [Bot Users](https://api.slack.com/bot-users) in Slack and create a bot user. You will need to write down the API token for that user so that we can use it in the bot app. In addition, you'll need to manually invite the bot user to the channels that you want to monitor.

I have created a [docker image](https://hub.docker.com/r/lekum/ansiblebot/) of the bot, that ships with Ansible, SSH and the bot code, that only needs the environment variable `SLACK_API_TOKEN` to run. Probably you'll want to map volumes from your machine to `/opt/bot` inside the container so that Ansible can have access to your inventory, config files and ssh keys.

Start the container and start managing your infrastructure from your favourite Slack client:

```
alejandro.guirao
11:26 AM !ansible 192.168.1.1 -a "ls -lh /var/log/" -s

ansiblebotBOT
11:26 AM Executing ansible 192.168.1.1 -a "ls -lh /var/log/" -s:
192.168.1.1 | SUCCESS | rc=0 >>
total 1.4G
drwxr-xr-x. 2 root     root      6 Sep 29  2014 anaconda
drwxr-x---. 2 root     root     94 Jan 16 21:05 audit
-rw-r--r--. 1 root     root   1.1K Sep  4 07:41 boot.log
-rw-------  1 root     utmp    768 Jan 13 11:41 btmp
-rw-------  1 root     utmp    768 Dec 10 12:20 btmp-20160101
```

Happy hacking!
