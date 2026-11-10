# Overview #

If you have already implemented Home Automation using the Tuya Zigbee with SmartLife, you will have noted that when your internet drops, you loose the ability to schedule devices to turn ON and OFF.

You will also realise that it would be nice to have the following:

- Dashboards! - Particulaly for Energy monitoring and graphing the temparatures from around the house.
- Overall device status
- Simple flexibility in how you manage your devices.

The goal of this project is to provide a CLI and GUI driven environment to control and monitor Tuya Zigbe devices via the gateway from a Linux command promp in a language such as PHP which is widly used, reltively secure and easy to program in.

## The setup Environment ##

This project assumes you have a typical home network, with a "spare" ethernet or wirelessly connected Linux Box, like Linux Mint or even UBUNTU.

You also need a working Tuya Zigbee network gateway hub. This hub will have connected itself to your WiFi during the setup/installation process and it might have an IP of 192.168.1.1 or 10.0.0.1 or something similer.

Your installation will need some devices, like wall switches, power plugs or temperature sensors etc and these are working via an App like smartlife.

## Programming Environment ##

Ths code in this project is implemented in basic PHP, version 8.2 or above (preferrably the latest).

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
* https://github.com/redphx/tuya-local-key-extractor - untested but looks promising.

Please note, I used the IoT platform and have not tested the above 4 options personaly.

# System Components #

I needed a dashboard, for this, I have found Tailwind CSS to offer a great CSS based GUI environment for rendering command and control dashboards for work.

I love linux so putting stuff in CRON to automate the backend of things seams to me to be perfectly manageable.

During development I wrote a number of small simple PHP programs to query and control the Gateway by issuing commands using the cloud interface first. Then I developed the local version of those programs to use the local ethernet link in my home office.

I put these small test programs in the "tools" directory. These include:

- device_on.php
- device_off.php
- get_status.php
- set_device_value.ph

This seamed like a simple no nonsense way to control devices. So theoretically, these programs alone should meet most peoples needs. For dimable or variable control devices, set_device_value.php seamed like a solution.

These programs also take command line options, usually -f <file>.data points to a name/value file specific to a device, while -d <device_id> will pass the specific device ID. The -d option will also create a basic <device_id>.data file that can be edited for enhanced functionality and then read back using a -f <file> option.


## The Gateway ##

To make these programs uniform, the common data like the gateway details is contained in a file called gateway.json, in it is one or more JSON arrays with gateway details.

While your SmartLife APP can only talk to 1 gateway, it seamed logical that you could have multiple gateways around your house to cater for more than usual devices or segregation of devices.


## The Scheduler ##

The scheduler is typically run from CRON every minute and looks for work to do. It will read the devices ".data" files to build an overall status of your home network.

The status data will be generated and saved into /opt/Simple_HA/status.json, if you run the Dashboard, it will read the status.json file and use it as needed to render it's displays.

*Updated 2026-11-10*
