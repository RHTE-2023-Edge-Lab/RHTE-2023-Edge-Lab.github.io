---
title: "Test the ESP8266 firmware"
draft: false
weight: 5
---

* Connect to your ESP8266 console

![PlatformIO console](/images/platformIO-esp-console.png)

* Place an RFID tag on the reader
* Check that the tag id is printed on the console
* Check that the tag is also sent over Wifi to the MQTT broker:

Go to the OpenShift Administrator console and click the command line terminal icon **>_** in the top-right corner to open a **Web Terminal**. Run the following commands:

```sh
# Get the load balancer URL generated from the service created
LOAD_BALANCER_URL=$(oc get svc mqtt-lb -ojsonpath="{.status.loadBalancer.ingress[0].hostname}")
# Subscribe to MQTT topic
mosquitto_sub -t esp8266-out -h ${LOAD_BALANCER_URL} -p 1883 -u admin -P public
```

You should see similar results as the following screenshot:

![PlatformIO result](/images/platformIO-esp-out-test.png)
