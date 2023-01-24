---
title: "Develop the ESP8266 firmware"
draft: false
weight: 5
---

In the left pane of VScode, click the top-left icon to diplay the project's files.

Replace the content of **src/main.cpp** with the following code.

**Warning**: you will have to adapt the code a little bit. See below for explanations.

You need to retrieve the load balancer url of the MQTT service.  
Go to the OpenShift Administrator console and click the command line terminal icon **>_** in the top-right corner to open a **Web Terminal**. Copy and paste this command into the Openshift Web Terminal. 
```shell
oc get svc mqtt-lb -ojsonpath="{.status.loadBalancer.ingress[0].hostname}"
```

Copy the result of the previous command and adapt the code bellow.

```cpp
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
const char *mqtt_broker = "<LB address for MQTT service>";
const char *mqtt_topic = "esp8266-out";
const char *mqtt_username = "admin";
const char *mqtt_password = "public";
const int mqtt_port = 1883;

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
