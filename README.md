# RaspiWiFi

RaspiWiFi is a program to **headlessly configure a Raspberry Pi's WiFi
connection** using using any other WiFi-enabled device (much like the way
a Chromecast or similar device can be configured).

It can also be used as a method to connect wirelessly point-to-point with your
Pi when a network is not available or you do not want to connect to one. Just
leave it in Configuration Mode, connect to the ```RaspiWiFi[xxxx] Setup``` access
point. The Pi will be addressable at ```10.0.0.1``` using all the normal methods you
might use while connected through a network.

RaspiWiFi has been **tested with the Raspberry Pi B+, Raspberry Pi 3, and Raspberry Pi Zero W**.

This repository was forked from [jasbur/RaspiWiFi](https://github.com/jasbur/RaspiWiFi). It was forked to implement additional features.

## Roadmap / Feature requests

- [ ] Modern, clean web ui with Bootstarp integration
- [ ] Captive portal: automatically open the web ui after connecting to the hotspot

## OS IMAGE USAGE

Just burn the ".IMG" file attached to this release to an 8GB+ SD card. Boot
your Raspberry Pi with the SD card and it will automatically boot into its AP
Host (broadcast) mode with an SSID based on a unique id (the last four of your
Pi's serial number). No input devices or displays necessary. Otherwise this is
a base install of the current Raspbian Stretch, up to date as of the date of
this release.

## SCRIPT-BASED INSTALLATION INSTRUCTIONS

Navigate to the directory where you downloaded or cloned RaspiWiFi

Run:

```bash
cd
git clone https://github.com/Rebel3D-CZ/RaspiWiFi.git
cd RaspiWiFi
sudo python3 initial_setup.py
```

This script will install all necessary prerequisites and copy all necessary
config and library files, then reboot. When it finishes booting it should
present itself in **"Configuration Mode"** as a WiFi access point with the
name ```RaspiWiFi[xxxx] Setup```.

The original RaspiWiFi directory that you ran the Initial Setup is no longer
needed after installation and can be safely deleted. All necessary files are
copied to ```/usr/lib/raspiwifi/ on setup```.

### CONFIGURATION

You will be prompted to set a few variables during the Initial Setup script:

1. **"SSID Prefix"** [default: "RaspiWiFi Setup"]: This is the prefix of the SSID
      that your Pi will broadcast for you to connect to during
      Configuration Mode (Host Mode). The last four of you Pi's serial number
      will be appended to whatever you enter here.
2. **"WPA Encryption"** [default: No]: If oyu enable this setting the Access Point 
      created during Configuration Mode will be encrypted using WPA2 encryption. 
      The prompt following this one will let you specify the Wireless Key to be 
      used. You can leave the password blank if you chose 'N' to this option. 
3. **"Auto-Config mode"** [default: n]: If you choose to enable this mode your Pi
      will check for an active connection while in normal operation mode (Client Mode).
      If an active connection has been determined to be lost, the Pi will reboot
      back into Configuration Mode (Host Mode) automatically.
4. **"Auto-Config delay"** [default: 15 seconds]: This is the time in consecutive
      seconds to wait with an inactive connection before triggering a reset into
      Configuration Mode (Host Mode). This is only applicable if the
      "Auto-Config mode" mentioned above is set to active.
5. **"Auto-Config after boot"** [default: 60 seconds]: This is the time in seconds
      since boot after which is auto-config disabled.
      This is ment to protect any running tasks before unexpected reboots when WiFi is lost.
      This is only applicable if the "Auto-Config mode" mentioned above is set to active.
      The value should be at least by 30s bigger than Auto-config delay.
6. **"Server port"** [default: 80]: This is the server port that the web server
      hosting the Configuration App page will be listening on. If you change
      this port make sure to add it to the end of the address when you're
      connecting to it. For example, if you speficiy 12345 as the port number
      you would navigate to the page like this: http://10.0.0.1:12345 If you
      leave the port at the default setting [80] there is no need to specify the
      port when navigating to the page.
7. **"SSL Mode"** [default: n]: With this option enabled your RaspiWifi
      configuration page will be sent over an SSL encrypted connection (don't
      forget the "s" when navigating to https://10.0.0.1:9191 when using
      this mode). You will get a certificate error from your web browser when
      connecting. The error is just a warning that the certificate has not been
      verified by a third party but everything will be properly encrypted anyway.

All of these variables can be set at any time after the initial setup has
been running by editing the ```/etc/raspiwifi/raspiwifi.conf```

## SET CONFIGURATION FOR WEB SITE

**Not mandatory**, but helpful to set the strings of the footer and the title of the web pages:
Create a config file (see below) and copy it to the right path.

```bash
sudo cp config.yaml  /usr/lib/raspiwifi/configuration_app/
```

## USAGE

Connect to the ```"RaspiWiFi[xxxx] Setup"``` access point using any other WiFi enabled
device.

1. Navigate to [10.0.0.1], [raspiwifisetup.com], or [idliketoconfigurethewifionthisdevicenowplease.com] (I was debating whether this
was funny or not and, yes, it was) using any web browser on the device you
connected with. (don't forget to manually start with [https://] when using SSL mode)
2. Select the WiFi connection you'd like your Raspberry Pi to connect to from
the drop down list and enter its wireless password on the page provided. If no
encryption is enabled, leave the password box blank. You may also manually
specify your network information by clicking on the "manual SSID entry ->" link.
3. Click the "Connect" button.
4. At this point your Raspberry Pi will reboot and connect to the access point
specified.

You can view the current WPA encryption settings and change them from the main Web 
Configuration interface. The current settings are visible in a panel in the upper 
left corner of the screen. If you click the values in this display you will be taken 
to a page where you can change them. If you change them your device will reboot to 
enable the new configuration. 

You can also use the Pi in a point-to-point connection mode by leaving it in
Configuration Mode. All services will be addresible in their normal way at
```10.0.0.1``` while connected to the ```"RaspiWiFi[xxxx] Setup"``` AP.



## RESETTING THE DEVICE

If **GPIO 18** is pulled HIGH for 10 seconds or more the Raspberry Pi will reset
all settings, reboot, and enter "Configuration Mode" again. It's useful to have
a simple button wired on GPIO 18 to reset easily if moving to a new location,
or if incorrect connection information is ever entered. Just press and hold for
10 seconds or longer.

You can also reset the device by running the ```manual_reset.py``` in the
```/usr/lib/raspiwifi/reset_device``` directory as root or with sudo.


## UNINSTALLATION

You can uninstall RaspiWiFi at any time by running:

```bash   
sudo python3 /usr/lib/raspiwifi/uninstall.py
```

You can also run it from the "libs/" directory from a fresh clone if you've 
installed from a previous version and don't have ```/usr/lib/raspiwifi/uninstall.py ```
available.

## CONFIGURATION

### Parameters for the web pages

It#s possible to set some elements of the web pages with the contents of a YAML file called **config.yaml**. This file is expected to be found in the ```configuration_app```folder. (This configuration file is excluded from git.)

Example file: 

```yaml
web:
    footer:
        vendor_name: New Startup Inc.
        version_nr: 1.0
        year: 2020
    
    title: NewStartup
```

These parameter set the first part of the HTML title tag as well as the footer string of the webpages.

## DEVELOPMENT

To make changes on the web pages you can run your code in a container. With VSCode and the [Remote Containers extension](https://github.com/Microsoft/vscode-remote-release) it is really easy. 

### Open your workspace in the container

Click the button in the lower left corner of the editor window to open the workspace in a container.

![Open in container](img/devcontainer1.png)

In the menu select the command ```Remote-Containers: Reopen in Container```.

![Menu to open in container](img/devcontainer2.png)

The project is configured to load the Python3 container. After the container started, you can open the termninal window of VSCode and run the following commands:

![Run the app in development mode](img/devcontainer_terminal.png)

You need a copy of the raspiwifi.conf in teh same folder (thi file is not under version control). Mind the command line option ```--test```!

Click the URL in the terminal window to redirect your browser to your web application.

