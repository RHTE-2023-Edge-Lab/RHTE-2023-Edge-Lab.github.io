+++
title = "Create Content"
date = 2022-12-13T15:22:04+01:00
weight = 5
chapter = true
pre = "<b>X. </b>"
+++

### Chapter 2



# RFID Reader with ESP8266

On that chapter we will configure our RFID reader module to comunicate with the ESP8266. and write the code to send the data to the MQTT broker.

## Hardware and Wiring

The RFID module communicates with the microcontroller using the SPI protocol, which requires four connections: MOSI (Master Out, Slave In), MISO (Master In, Slave Out), SCK (Serial Clock), and SS (Slave Select). The RFID reader also has a reset pin (RST) and a data pin (SDA).

To connect the RFID reader to the ESP8266, you will need to use a breadboard and some wires. Follow the wiring diagram provided earlier in the tutorial to connect the RFID reader to the ESP8266:

| Signal | Pin |
|--------|-----|
| SDA    | D8  |
| MOSI   | D7  |
| MISO   | D6  |
| SCK    | D5  |


## Software

To set up the software for this tutorial, you will need to follow these steps:


Install a code editor such as VSCode. This will be the tool you use to write and edit your code.

Install the PlatformIO IDE plugin for your code editor. PlatformIO is a tool that simplifies the process of building and uploading code to microcontrollers.

To add the PlatformIO plugin to Visual Studio Code and create a new project, follow these steps:


1. Open Visual Studio Code and click the "Extensions" icon in the left sidebar.
2. In the search bar, type "platformio" and press Enter.
3. Click the "Install" button for the "PlatformIO IDE" extension.
4. After the extension is installed, click the "Reload" button in the notification that appears.
5. Click the "PlatformIO" icon in the left sidebar.
6. In the PlatformIO panel, click the "New Project" button.
7. Select the following options:
   - Board Name: Wemos D1 R2 and mini
   - Framework: Arduino
8. In the PlatformIO home screen, add the MFRC522 library by Miguel André Balboa. You can find it by searching for the header file: `MFRC522.h`. This library provides functions for communicating with the RFID reader.
9.  Add the PubSubClient library by Nick O'Leary. This library provides functions for working with the MQTT protocol, which we will use to send data from the ESP8266 to a message broker.


If you are on linux, fix permissions on /dev/ttyUSB0 by running the following command:

```shell
sudo usermod -a -G dialout $(id -un)
```

This command adds your user to the dialout group, which allows you to access the serial port.

## Code

For the main source file (src/main.main.cpp), use the following code:

```shell
#include <Arduino.h>
#include <SPI.h>
#include <MFRC522.h>
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

MFRC522 mfrc522;  // Initialize the RFID reader object
WiFiClient espClient;  // Initialize the WiFi client object
PubSubClient client(espClient);  // Initialize the MQTT client object

void setup()
{
  Serial.begin(9600);  // Initialize serial communication with the PC
  while (!Serial);  // Do nothing if no serial port is opened (added for Arduinos based on ATMEGA32U4)
  Serial.println("Initializing...");
  SPI.begin();  // Initialize the SPI bus
  mfrc522.PCD_Init();  // Initialize the RFID reader
  delay(4);  // Optional delay
  
  // Check if the RFID reader is ready
  if (!mfrc522.PCD_PerformSelfTest()) {
    Serial.println("MFRC522 not ready!");
  }
  Serial.println("Ready!");

  // Initialize WiFi
  WiFi.mode(WIFI_STA);
  WiFi.begin("YourSSID", "REDACTED");  // Replace with your own WiFi credentials
  Serial.print("Connecting to WiFi...");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();
  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
  randomSeed(micros());  // Initialize the random number generator

  // Initialize MQTT
  client.setServer("test.mosquitto.org", 1883);  // Set the MQTT broker address
```