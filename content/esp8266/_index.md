+++
title = "Program the ESP8266"
weight = 4
chapter = true
pre = "<b>A & B. </b>"
+++

# Program the ESP8266

## 0. Fix permissions on /dev/ttyUSB0

The /dev/ttyUSB0 device belongs to the "dialout" group. Therefore, your user should belong to this group to configure the ESP8266. Run the following command and restart your session to fix the permissions.

```sh
sudo usermod -a -G dialout $(id -un)
```

## 1. Duo A & B - Connect your ESP8266

Connect ESP8266 to your computer via USB connector.

![ESP8266 USB](/images/esp8266-usb.jpeg)

## 2. Duo A & B - Create new project in PlaformIO VSCode plugin

![PlatformIO Home](/images/platformIO-home.png)

![PlatformIO New project](/images/platformIO-new-project1.png)

![PlatformIO New project](/images/platformIO-new-project2.png)

![PlatformIO New project](/images/platformIO-new-project3.png)

## 3. Duo A & B - Add specific libraries in your new create project

* Library **MFRC522 by Miguel André Balboa** : Arduino RFID Library for MFRC522 (SPI). Read/Write a RFID Card or Tag using the ISO/IEC 14443A/MIFARE interface.  
  
  
* Library **PubSubClient by Nick O'Leary** : A client library for MQTT messaging. MQTT is a lightweight messaging protocol ideal for small devices. This library allows you to send and receive MQTT messages. It supports the latest MQTT 3.1.1 protocol and can be configured to use the older MQTT 3.1 if needed. It supports all Arduino Ethernet Client compatible hardware, including the Intel Galileo/Edison, ESP8266 and TI CC3000.  


![PlatformIO add library](/images/platformIO-add-library1.png)

![PlatformIO add library](/images/platformIO-add-library2.png)

![PlatformIO add library](/images/platformIO-add-library3.png)

![PlatformIO add library](/images/platformIO-add-library4.png)

![PlatformIO add library](/images/platformIO-add-library5.png)

## 4. Duo A - Deploy, build and upload code on your ESP8266 dedicated for entrence packets 

### Adapt this code in the src/main.cpp file of your plarformIO project

```sh
#include <Arduino.h>
#include <SPI.h>
#include <MFRC522.h>
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

MFRC522 mfrc522;
WiFiClient espClient;
PubSubClient client(espClient);

//WIFI
const char* ssid = "<wifi_ssid>";  //  SSID Wifi
const char* password = "<wifi_password>";  //mot de passe Wifi


// MQTT Broker
const char *mqtt_broker = "<AWS LB address for MQTT service>";
const char *mqtt_topic = "esp8266-in";
const char *mqtt_username = "<MQTT user>";
const char *mqtt_password = "<MQTT password>";
const int mqtt_port = <MQTT port>;

void setup()
{
  Serial.begin(9600);     // Initialize serial communications with the PC
  while (!Serial);        // Do nothing if no serial port is opened (added for Arduinos based on ATMEGA32U4)
  Serial.println("Initializing...");
  SPI.begin();            // Init SPI bus
  mfrc522.PCD_Init();     // Init MFRC522
  delay(4);               // Optional delay.

  if (!mfrc522.PCD_PerformSelfTest()) {
    Serial.println("MFRC522 not ready !");
  }
  Serial.println("Ready !");

  // Wifi initialization
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.print("Connecting to Wifi...");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();
  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
  randomSeed(micros());

  // MQTT initialization
  client.setServer(mqtt_broker, mqtt_port);
}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str(), mqtt_username, mqtt_password)) {
      Serial.println("connected to MQTT Broker");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void loop()
{
  // MQTT connection to the broker + protocol handling
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  // Reset the loop if no new card present on the sensor/reader. This saves the entire process when idle.
  if ( ! mfrc522.PICC_IsNewCardPresent()) {
    return;
  }

  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) {
    return;
  }

  // Compute and print UID
  char uid[30];
  char * buffer = uid;
  for (byte i = 0; i < mfrc522.uid.size; i++) {
    int n = sprintf(buffer, "%s%02x", i > 0 ? ":" : "", mfrc522.uid.uidByte[i]);
    if (n > 0) {
      buffer += n;
    }
  }
  Serial.println(uid);
  client.publish(mqtt_topic, uid);
  delay(500);
}
```

### Compile your code

![PlatformIO build](/images/platformIO-esp-build.png)

### Flash your ESP8266 with your code

![PlatformIO write](/images/platformIO-esp-write.png)

### Connect to your ESP8266 console

![PlatformIO console](/images/platformIO-esp-console.png)

### Result

![PlatformIO result](/images/platformIO-esp-test.png)


## 4. Duo B - Deploy, build and upload code on your ESP8266 dedicated for output packets 

### Adapt this code in the src/main.cpp file of your plarformIO project

```sh
#include <Arduino.h>
#include <SPI.h>
#include <MFRC522.h>
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

MFRC522 mfrc522;
WiFiClient espClient;
PubSubClient client(espClient);

//WIFI
const char* ssid = "<wifi_ssid>";  //  SSID Wifi
const char* password = "<wifi_password>";  //mot de passe Wifi


// MQTT Broker
const char *mqtt_broker = "<AWS LB address for MQTT service>";
const char *mqtt_topic = "esp8266-out";
const char *mqtt_username = "<MQTT user>";
const char *mqtt_password = "<MQTT password>";
const int mqtt_port = <MQTT port>;

void setup()
{
  Serial.begin(9600);     // Initialize serial communications with the PC
  while (!Serial);        // Do nothing if no serial port is opened (added for Arduinos based on ATMEGA32U4)
  Serial.println("Initializing...");
  SPI.begin();            // Init SPI bus
  mfrc522.PCD_Init();     // Init MFRC522
  delay(4);               // Optional delay.

  if (!mfrc522.PCD_PerformSelfTest()) {
    Serial.println("MFRC522 not ready !");
  }
  Serial.println("Ready !");

  // Wifi initialization
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.print("Connecting to Wifi...");
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println();
  Serial.print("Connected, IP address: ");
  Serial.println(WiFi.localIP());
  randomSeed(micros());

  // MQTT initialization
  client.setServer(mqtt_broker, mqtt_port);
}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str(), mqtt_username, mqtt_password)) {
      Serial.println("connected to MQTT Broker");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void loop()
{
  // MQTT connection to the broker + protocol handling
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  // Reset the loop if no new card present on the sensor/reader. This saves the entire process when idle.
  if ( ! mfrc522.PICC_IsNewCardPresent()) {
    return;
  }

  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) {
    return;
  }

  // Compute and print UID
  char uid[30];
  char * buffer = uid;
  for (byte i = 0; i < mfrc522.uid.size; i++) {
    int n = sprintf(buffer, "%s%02x", i > 0 ? ":" : "", mfrc522.uid.uidByte[i]);
    if (n > 0) {
      buffer += n;
    }
  }
  Serial.println(uid);
  client.publish(mqtt_topic, uid);
  delay(500);
}
```

### Compile your code

![PlatformIO build](/images/platformIO-esp-build.png)

### Flash your ESP8266 with your code

![PlatformIO write](/images/platformIO-esp-write.png)

### Connect to your ESP8266 console

![PlatformIO console](/images/platformIO-esp-console.png)

### Result

![PlatformIO result](/images/platformIO-esp-test.png)