+++
subtitle = ""
title = "Building a Slack button with ESP8266/Arduino"
bigimg = ""
date ="2017-02-08T22:53:27+01:00"

+++

I have recently discovered the powerful [ESP8266](https://en.wikipedia.org/wiki/ESP8266)-based development boards. These boards have built-in WiFi connectivity and many GPIOs, low consumption and can be programmed in many different ways. In this article I will show how to build a *smart button* that sends a message to a [Slack](https://slack.com/) channel, using a [Wemos D1 mini pro](https://www.wemos.cc/product/d1-mini-pro.html).

<!-- TEASER_END -->

This board can be programmed in many different ways. In this article, I will show how to do it using the Arduino-compatible programming interface. To make it really easy, we will use the [PlatformIO](http://platformio.org/) command line utility. I will suppose that you have it installed and you are able to run the `platformio` command.

First, we will initialize a folder for the project with the command:

```bash
platformio init -b d1_mini
```

You should now have this folder structure:

```bash
.
├── lib
│   └── readme.txt
├── platformio.ini
└── src
```

You can verify that the project is configured to use the Wemos D1 mini board using the arduino programming interface:

```bash
$ cat platformio.ini

; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; http://docs.platformio.org/page/projectconf.html

[env:d1_mini]
platform = espressif8266
board = d1_mini
framework = arduino
```

We will place the sketch that we will develop in the `src` folder (e.g. `src/slack.ino`). In order to compile and upload it to the board, connect it via USB to the computer and run:

```bash
platformio run -t upload
```

If you need to debug using the USB connection, you can use:

```bash
platformio device monitor
```

The schematic of the circuit that we will be building is the following:

![Schematic](https://github.com/lekum/esp8266sketches/raw/master/slack/circuit.png)

The idea of the circuit is that, when someone pushes the button, the board will make an HTTP POST to the Slack API, writing a message in some Slack channel, and giving visual feedback of the success lighting a LED. This could be used, for example, to make a broadcast notification of any event, such an alarm, that can be triggered easily without having to use any other device.

The Wemos D1 must be powered, for instance, via a USB charger or external battery. We don't need to put a resistor in a serial connection with the LED because we will be using the `INPUT_PULLUP` built-in resistor of the pin that will drive the current to the LED.

In order to be able to POST to the Slack channel, we will need to enable an Incoming Webhook. Please refer to the [Slack documentation](https://api.slack.com/incoming-webhooks) in order to do that. You will need these values for the sketch:

```c
/*
 SLACK CONFIGURATION
 */

const String slack_hook_url = "<SLACK_HOOK_URL>";
const String slack_icon_url = "<SLACK_ICON_URL>";
const String slack_message = "<SLACK_MESSAGE>";
const String slack_username = "<SLACK_USERNAME>";
```

In order to connect to the WiFi, we will need to populate the sketch with the SSID and password:

```c
/*
 WIFI CONFIGURATION
 */
char SSID[] = "<YOUR_SSID>";
char pwd[] = "<YOUR_PASSWORD>";
```

In the `setup()` part of the sketch, we will initialize the pins, the Serial connection (to enable USB debugging) and we will connect to the WiFi:

```c
void setup()
{
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  Serial.begin(115200);
  Serial.println();

  WiFi.begin(SSID, pwd);

  Serial.print("Connecting");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();

  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());

[...]
}
```

In addition, adding this special code snippet, we can enable Over The Air (OTA) updates of the sketch, so that we can upload the code via WiFi:

```c
  // OTA setup
  ArduinoOTA.onStart([]() {
    Serial.println("Start");
  });
  ArduinoOTA.onEnd([]() {
    Serial.println("\nEnd");
  });
  ArduinoOTA.onProgress([](unsigned int progress, unsigned int total) {
    Serial.printf("Progress: %u%%\r", (progress / (total / 100)));
  });
  ArduinoOTA.onError([](ota_error_t error) {
    Serial.printf("Error[%u]: ", error);
    if (error == OTA_AUTH_ERROR) Serial.println("Auth Failed");
    else if (error == OTA_BEGIN_ERROR) Serial.println("Begin Failed");
    else if (error == OTA_CONNECT_ERROR) Serial.println("Connect Failed");
    else if (error == OTA_RECEIVE_ERROR) Serial.println("Receive Failed");
    else if (error == OTA_END_ERROR) Serial.println("End Failed");
  });
ArduinoOTA.begin();
```

The main `loop()` will just start the OTA service, read the button values and call the `postMessageToSlack()` function if pressed, lighting the LED if the result is `true`:

```c
void loop()
{
  // Start handling OTA updates
  ArduinoOTA.handle();

  int buttonVal = digitalRead(buttonPin);
  if (!buttonVal) {
    bool ok = postMessageToSlack(slack_message);
    if (ok) {
      digitalWrite(ledPin, HIGH);
      delay(1000);
    }
  }
  digitalWrite(ledPin, LOW);
  delay(10);
}
```

The `postMessageToSlack()` function performs the HTTP POST constructing the request and sending it using the `WiFiClientSecure` and returns a `bool` that is `true` when the status code is `200 OK`:

```c
bool postMessageToSlack(String msg)
{
  const char* host = "hooks.slack.com";
  Serial.print("Connecting to ");
  Serial.println(host);

  // Use WiFiClient class to create TCP connections
  WiFiClientSecure client;
  const int httpsPort = 443;
  if (!client.connect(host, httpsPort)) {
    Serial.println("Connection failed :-(");
    return false;
  }

  // We now create a URI for the request

  Serial.print("Posting to URL: ");
  Serial.println(slack_hook_url);

  String postData="payload={\"link_names\": 1, \"icon_url\": \"" + slack_icon_url + "\", \"username\": \"" + slack_username + "\", \"text\": \"" + msg + "\"}";

  // This will send the request to the server
  client.print(String("POST ") + slack_hook_url + " HTTP/1.1\r\n" +
               "Host: " + host + "\r\n" +
               "Content-Type: application/x-www-form-urlencoded\r\n" +
               "Connection: close" + "\r\n" +
               "Content-Length:" + postData.length() + "\r\n" +
               "\r\n" + postData);
  Serial.println("Request sent");
  String line = client.readStringUntil('\n');
  Serial.printf("Response code was: ");
  Serial.println(line);
  if (line.startsWith("HTTP/1.1 200 OK")) {
    return true;
  } else {
    return false;
  }
}
```

The code of the complete sketch can be found [here](https://github.com/lekum/esp8266sketches/tree/master/slack). If you want to perform OTA updates to the sketch, just plug the USB and run the serial connector monitor to see the IP of the Wemos D1, then add this to the `[env:d1_mini]` section of the `platformio.ini` file:

```ini
upload_port=<IP>
```

After that, you can unplug the USB, and upload the code using the same `platformio run -t upload` command.

Enjoy!
