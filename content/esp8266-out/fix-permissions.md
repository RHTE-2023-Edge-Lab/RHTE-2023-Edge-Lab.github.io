---
title: "Fix permissions on /dev/ttyUSB0 (Linux only)"
draft: false
weight: 1
---

The /dev/ttyUSB0 device belongs to the "dialout" group. Therefore, your user should belong to this group to configure the ESP8266.

Run this command:

```sh
sudo usermod -a -G dialout $(id -un)
```

Then, close Visual Studio Code and re-run it with the new group membership:

```sh
su -c code $(id -un)
```
