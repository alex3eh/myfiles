# Shelly Binding (org.openhab.binding.shelly)

This openHAB 2 Binding implements control for the Shelly series of devices.
This includes sending commands to the devices as well as reading the device status and sensor data.

Author: Markus Michels (markus7017)
Check  https://community.openhab.org/t/shelly-binding/ for more information, questions and contributing ideas. Any comments are welcome!

Also check section **Additional Information** at the end of the document.
This includes some general comments, information how to debug and request new features.

---

## Installing DEV/SNAPSHOT version

IMPORTANT FOR 2.4 & 2.5M3-M6 users / Beta users:

The 2.5 release version of the binding does no longer include the Californium libs. Those libraries are required to implement the CoIoT / Coap protocol. Those need to be installed on pre-2.5 final installations. openHAB 2.4 is still supported (at the moment), but 2.4 and 2.5 pre-releases require the installation of Gson 2.8.5 and the Californium libs.

I'm waiting on the information how the official path going forward will be. At the moment it looks like having
- one repo/branch for openHAB 3.0 and
- one branch for 2.5 updates (backports from 3.0)

This will allow to provide fixes and enhancements for the time given until openHAB 3.0 becomes available (at least stable milestone builds).

At the moment I have 3 build locations
- The official openHAB 2.5 install package providing the released version when installing with PaperUI
- The official [2.5 SNAPSHOT build](https://openhab.jfrog.io/openhab/libs-pullrequest-local/org/openhab/addons/bundles/org.openhab.binding.shelly/2.5.0-SNAPSHOT/). Please be aware of limited stability.
- My private [2.5 DEV build](https://github.com/markus7017/myfiles). This is the latest build, could be instable. Once initial testing has been done I checkin to the SNAPSHOT repo.

**If you want to use the version released with openHAB 2.5 final**
- The final release could be installed as usual using PaperUI:Addons:Bindings:Shelly
- This version works fine. However, in between some bugs (e.g. LOW_BATTERY alarm for sensor devices, input channels for Dimmers) has been fixed and new features are implemented (e.g. German translation). If you want to get access to these you need to switch to the dev/snapshot build - see below:

If you want to use the SNAPSHOT/DEV build you can NOT install this using PaperUI. Make sure that the release version is not installed. You can NOT run the SNAPSHOT on top of the version you install with PaperUI.

**2.4 & 2.5M3-M6 and DEV/SNAPSHOT users,** which want to use the latest 2.5 DEV/SNAPSHOT builds:
- Stop openHAB and wait a minute
- Run "bundle:list | grep Gson" on the openHAB console and check if Gson 2.8.5 (or newer) is installed (sometimes version 2.7 is installed, which doesn't solve the dependencies).
If not: Download the gson from http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/gson-2.8.5.jar
and copy the jar to the OH addons folder
- Now install the Californium libs
Run "bundle:list | grep Californium" and check if the libs are already installed (e.g. when the Tradfi binding 2.5 is also installed), otherwise
Download https://repo1.maven.org/maven2/org/eclipse/californium/californium-core/2.0.0/californium-core-2.0.0.jar and copy to addons folder
Download https://repo1.maven.org/maven2/org/eclipse/californium/element-connector/2.0.0/element-connector-2.0.0.jar and copy to addons folder

**Install DEV/SNAPSHOT build of the binding**
- Perform the above mentioned installation steps.
- If you want to use the official SNAPSHOT release
download https://openhab.jfrog.io/openhab/libs-pullrequest-local/org/openhab/addons/bundles/org.openhab.binding.shelly/2.5.0-SNAPSHOT/org.openhab.binding.shelly-2.5.0-SNAPSHOT.jar
OR
If you want to use the latest DEV version download https://github.com/markus7017/myfiles/blob/master/org.openhab.binding.shelly-2.5.0-SNAPSHOT.jar?raw=true
Usually the DEV version is newer than the SNAPSHOT release from the above link.
- Copy the downloaded jar into openHAB's addons folder.
- Start openHAB and wait until it is fully initialized

If everything was install correct a "bundle:list" output show be similar to this:

```csv
245 │ Installed │  80 │ 2.8.5                  │ Gson
246 │ Installed │  80 │ 2.0.0                  │ Californium (Cf) Core
247 │ Installed │  80 │ 2.0.0                  │ Californium (Cf) Element Connector
248 │ Installed │  80 │ 2.5.0.201912112158     │ openHAB Add-ons :: Bundles :: Shelly Binding
```

Please let me know if you have problems installing the new build or this doc can be improved.

## Updating Alpha/Beta versions

Channel definitions are subject to change with any alpha or beta release. Please make sure to **delete all Shelly things before updating*** the binding and clean out the JSON DB:

- **remove all shelly entries from paperui**
- stop oh2 service
- openhab-cli clear-cache
- copy jar into addons (set correct permission)
- start oh2 service
- **re-discover things**
- the channel/item linkage should be restored automatically

If you hit a problem make sure to post a TRACE log (or send PM) so I could look into the details.

### Instalation

As described above the binding will be installed by copying the jar into the addons folder of your OH installation.
Once a stable state is reached the binding may become part of the openHAB 2.5 distribution, but this will take some time.
The binding was developed an tested on OH version 2.4 and 2.5. 
Please post an info if you also verified compatibility to version 2.3.
However, this release is not officially supported.

# Additional Notes

## General

* You should use firmware version 1.5.2 or never.
It might be that the binding is working with older versions, but this was never tested.
List of Firmware Versions for the different devices could be found here: https://api.shelly.cloud/files/firmware


* If you gave multiple network interfaces you should check openHAB's default setting.

Open PaperUI and go to Configuration:System-:Network Settings and verify the selected interface. 
If the Shelly devices are not on the same network you could try to add them manually.
However, devices in different networks have not been tested yet (please post a comment in the community thread if you are successful).

## Reporting a problem/bug

If you encounter a problem you could put the device into DEBUG or TRACE mode

- open OH console (execute "openhab-cli console")
- set the debug level ("log:set DEBUG org.openhab.binding.shelly")
- issue command or wait until problem occurs
- post an extract of openhab.log to the community thread (or send the author a PM - make sure the log extract has enough information, some more lines are fine)

## Feature Request

Any comment or feature request is welcome. Post the idea to the community thread, all of us will benefit.

## Other devices

The thing definition of the following devices is primarily.
If you have one of those devices send a PM to marks7017 and we could work on the implementation/testing.

- thing-type: shellysmoke
- thing-type: shellysense
- thing-type: shellyplug

## Supporting new devices

You could help to integrate and support new devices. In general the following information is a good start

- open a browser and issue the following urls
- http://&lt;device ip&gt;/settings
- http://&lt;device ip&gt;/status

once basic discovery is implemented the Coap Discription could be discovered

- enable CoIoT events within the thing configuration
- open the thing properties ([Show Properties])
- and copy&amp;paste the coapDescr property

post this information in the community thread or send a PM to the author.
Depending on the device type and complexity of the integration you should be prepared to run test cycles with snapshort binds of the binding incl. back and forth communication with the author. 

