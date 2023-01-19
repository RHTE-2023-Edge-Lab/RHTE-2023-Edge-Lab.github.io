---
title: "Linux"
draft: false
weight: 2
---

## OpenShift CLI

Install VScode.

```sh
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
dnf check-update
sudo dnf install code
```

Install the [PlatformIO IDE plugin](https://docs.platformio.org/en/latest/integration/ide/) for VScode with the following command:

```sh
code --install-extension platformio.platformio-ide
```
