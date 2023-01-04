---
title: "Linux"
draft: false
weight: 2
---

## OpenShift CLI

Install the OpenShift CLI.

```sh
curl -sSfL https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.11.21/openshift-client-linux.tar.gz | tar -zx -C /usr/local/bin oc kubectl
```

Install git.

```sh
sudo dnf install git
```

Install VScode.

```sh
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
dnf check-update
sudo dnf install code
```

Open VScode and install the [PlatformIO IDE plugin](https://docs.platformio.org/en/latest/integration/ide/).

