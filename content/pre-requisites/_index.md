+++
title = "Pre Requisites"
date = 2022-12-13T15:22:04+01:00
weight = 1
chapter = true
pre = "<b>1. </b>"
+++

### Chapter 1

# Pre-requisites

In order to run that lab, you will need to install some tools on your laptop :
- oc cli
- git
- vscode (or your favorite IDE)
- platformIO IDE plugin (https://docs.platformio.org/en/latest/integration/ide/)


# Setup platformIO IDE plugin

Once platformIO IDE plgin installed, create a new project :
- Board Name: Espressif ESP32 Dev Module
- Framework: Arduino

In PIO Home, add the library MFRC522 by Miguel André Balboa (search for header:MFRC522.h).

Add the library PubSubClient by Nick O'Leary.

