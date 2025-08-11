+++
bigimg = ""
date = "2017-03-01T19:26:56+01:00"
subtitle = ""
title = "Using adafruit-ampy for the ESP8266"

+++

I recently wrote a [blog post](https://lekum.org/post/micropython_slack_button/) about how to work with MicroPython on the ESP8266. In that post, I used a tool named [webrepl](https://github.com/micropython/webrepl) to connect to the board and upload the Python code over the air.

That could be a feasible workflow once the ESP8266 is already in a WiFi we can access from our laptop, but sometimes that is not the case. And, of course, the first time we need to connect the board to the WiFi, which needs to be done over the serial connection.

I have recently discovered a very handy tool written by [Adafruit](https://www.adafruit.com/) called [ampy](https://github.com/adafruit/ampy). It works over the serial connection and allows an easier (on my view) and faster workflow.

In order to use it, just install it (preferably inside a [virtualenv](https://docs.python.org/3/library/venv.html)):

```bash
pip install adafruit-ampy
```

For ease of use, we will define an environment variable with the serial port:

```bash
export AMPY_PORT=/dev/ttyUSB0
```

And then we can use the `ampy` command:

```bash
$ ampy
Usage: ampy [OPTIONS] COMMAND [ARGS]...

  ampy - Adafruit MicroPython Tool

  Ampy is a tool to control MicroPython boards over a serial connection.
  Using ampy you can manipulate files on the board's internal filesystem and
  even run scripts.

Options:
  -p, --port PORT  Name of serial port for connected board.  Can optionally
                   specify with AMPY_PORT environemnt variable.  [required]
  -b, --baud BAUD  Baud rate for the serial connection (default 115200).  Can
                   optionally specify with AMPY_BAUD environment variable.
  --version        Show the version and exit.
  --help           Show this message and exit.

Commands:
  get    Retrieve a file from the board.
  ls     List contents of a directory on the board.
  mkdir  Create a directory on the board.
  put    Put a file or folder and its contents on the...
  reset  Perform soft reset/reboot of the board.
  rm     Remove a file from the board.
  rmdir  Forcefully remove a folder and all its...
  run    Run a script and print its output.
```

We can list the contents of the filesystem, create directories, upload and download files, and even run scripts directly. The latter creates a very interesting  interactive workflow for rapid iterations. Kudos to [@tdicola](https://twitter.com/tdicola) for the tool!
