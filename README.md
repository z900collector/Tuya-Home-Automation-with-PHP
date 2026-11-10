# Simple_HA #

## Overview ##

If you have already implemented Home Automation using the Tuya Zigbee with SmartLife, you will have noted that when your internet drops, you loose the ability to schedule devices to turn ON and OFF.

You will also realise that it would be nice to have the following:

- Dashboards! - Particulaly for Energy monitoring and graphing the temparatures from around the house.
- Overall device status
- Simple flexibility in how you manage your devices.

The goal of Simple_HA is to provide a CLI and GUI driven environment to control and monitor Tuya Zigbe devices via the gateway from a Linux command promp in a language such as PHP which is widly used, reltively secure and easy to program in.


## The setup Environment ##

Simple_HA is implemented in basic PHP, version 8.2 or above (preferrably the latest).

It runs on a Linux Box, my test bed is Mint Linux but it will work on any Red Hat, Rocky Linux or Oracle Linux platform and if the platform can run PHP then pretty much anything that uses systemd for service control.

Access to the local gateway via the local Wi-Fi TCP/IP network running in your house is required. No external internet is required.
Your gateway will have an IP address assigned to it and communications is via a port like 6668.

## The "Local Key" ##

There will be a "Local Key" the gateway uses to secure the symmetric encryption the devices use to talk to the gateway.

The most reliable method to obtain a Tuya Local Key is through the Tuya IoT Developer Platform.
You must create a separate developer account at iot.tuya.com, link your Smart Life or Tuya app account to a Cloud Development project, and then use the API Explorer.
Navigate to Device Management > Query Device Details (for the Gateway), select the device Id or enter your device’s Device ID, and the response will contain the local_key field. 

Alternative methods include:

* TinyTuya Wizard: Run python -m tinytuya wizard in your command line after installing the TinyTuya library; this tool automates the cloud API request and network scanning to output a JSON file with all device IDs and local keys.
* tuya-local Cloud-Assisted Setup: Some Home Assistant integrations (like tuya-local) allow you to log in directly via the Smart Life app without a developer account, automatically retrieving the key for local control.
* QR Login Tools: Community tools (such as tuya-local-key) allow you to scan a QR code generated from the Smart Life app to extract local keys without developer credentials.
* Rooted Android Extraction: On a rooted device, you can read the local_key directly from the Smart Life app’s local configuration files using dedicated extraction scripts.


