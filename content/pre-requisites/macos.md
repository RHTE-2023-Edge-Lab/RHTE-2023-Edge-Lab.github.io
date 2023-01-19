---
title: "MacOS"
draft: false
weight: 3
---

Install the OpenShift CLI.

```sh
curl -sSfL https://mirror.openshift.com/pub/openshift-v4/clients/ocp/stable/openshift-client-mac.tar.gz | tar -zx -C /usr/local/bin oc kubectl
```

Install git.

```sh
brew install git
```

Install mosquitto (for MQTT tests).

```sh
brew install mosquitto
```

Go to the VScode [download page](https://code.visualstudio.com/download), download the MacOS package and install it.

Open VScode and install the [PlatformIO IDE plugin](https://docs.platformio.org/en/latest/integration/ide/).

![Install1](/images/install_platformio_1.png)

![Install2](/images/install_platformio_2.png)