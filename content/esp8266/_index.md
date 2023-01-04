+++
title = "Program the ESP8266"
weight = 4
chapter = true
pre = "<b>A & B. </b>"
+++

### Duo A & B

# Program the ESP8266

## 1. Connect your ESP8266

TODO

## 2. Create new project in PlaformIO VSCode plugin

TODO

## 3. Add specific libraries in your new create project

TODO

## 4. Deploy, build and upload code on your ESP8266

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
const char *mqtt_topic = "<MQTT addressName>";
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
