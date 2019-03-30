# Homeassistant

Here's my [Home Assistant](https://home-assistant.io/) configuration. I have installed HASS on an old HP laptop for now. The laptop is running as a "homeserver". I am currently running Ubuntu 18.04 LTS on the laptop. HASS as well as all supporting applications (and some additional non HASS related applications) are running in Docker containers. 

I plan to move everything to a dedicated NUC in time. Docker should make this simple. 

I'm relatively new to HASS coming from a Domoticz RPi environment. That's why the two are running in parallel at the moment. This will change once HASS is fully operational. I spent quite some time to get the two working together.  

I regularly update my configuration files as HASS is still heavily under development.

## Things that I run on my Homeserver (all in Docker containers)

* [Home Assistant](https://home-assistant.io/)
* [Zigbee2MQTT](https://koenkk.github.io/zigbee2mqtt/), great development for getting rid of most of the propietary bridges. I use it for all my ZigBee devices except for Hue.
* [Mosquitto](https://mosquitto.org/), my favorite MQTT Broker. Used for exchanging data between a lot of applications and from the outside world (through the reverse proxy).
* [Domoticz](https://www.domoticz.com/), for test mainly, not really made for containerizing so stopped moving there with Domoticz.
* [InfluxDB](https://www.influxdata.com/), a time series database. Installed, tested with Domoticz but not used yet.
* [Grafana](https://grafana.com/), analytics and monitoring. Installed, tested with Domoticz but not used yet.
* [Traefik](https://traefik.io/), reverse proxy for secure access from the outside world.
* [Unifi Controller](https://www.ui.com/), for managing my Ubiquiti Access points.
* [Portainer](https://www.portainer.io/), makes managing my Docker containers easy.  

## Things planned

Many ideas. Too little time!

## Devices and services that I use

* Zwave:
  * [Aeotec Z-Stick Gen5](https://aeotec.com/z-wave-usb-stick) Z-Wave controller.
  * [Heatit Thermostat](https://www.heatit.com/heating-control/floor-heating-thermostats/heatit-z-wave-thermostat/) Z-Wave thermostat for controlling the electrical heating in one room.
  * [Neo Coolcam sensors](https://www.szneo.com/) Motion sensors and door/window sensors. Available at Ali Express for competitive prices.
  * [Fibaro smoke sensors](https://www.fibaro.com/en/) The best looking smoke detectors on the market (IMHO).

* Zigbee:
  * [Xiaomi Aqara bridge](https://www.aliexpress.com) The bridge is connected to Domoticz. As HASS is running inside a container, the bridge won't connect (I don't want HASS to run as host on Docker). Sensor values are exchanged through MQTT for now. When I have a better zigbee2mqtt controller (with better radio sensitivity), I will move all over to HASS and retire the bridge (and the privacy unfrienly Chinese app).
  * [Xiaomi Aqara sensors](https://www.aliexpress.com) I use motion sensors, door/window sensors, temp/hum/pressure sensors, a magic cube (nice gadget!), buttons. Some sensors are connected to HASS (through Zigbee2mqtt), others are still connected to the Aqara bridge.
  * [Philips Hue](https://www2.meethue.com) Philips Hue bulbs used in the house. Operated through the Hue Bridge as I use the scenes in the Bridge for setting the mood.
  * [Tradfri](https://www.ikea.com) Cheap ZigBee compatible smart home devices. Currently I only use the Wireless control outlets for the more critical switches (replacement of Click-On-Click-Off units).   

* MySensors platform:
  * [MySensors](https://www.mysensors.org/) The MySensors platform offers a strong solution for developing highly customized sensors and actors. I have developed a custom water meter sensor to read my dumb water meter values. Currently still connected to Domoticz.

* MQTT:
  * [OwnTracks](https://home-assistant.io/components/device_tracker.owntracks/) Presence detection is an important part of the home automation. OwnTracks is used for Geolocation over MQTT.
  * Interfacing to my Domoticz installation, used for controlling devices that are behind my Domoticz installation for the time being (mainly Click-On-Click-Off kind of devices).

* Wi-Fi and network:
  * [Netgear Nighthawk](https://www.netgear.nl/home/products/networking/wifi-routers/R7000.aspx), the center of all the networking activities in the house. Connects to all devices that require wired networking and to the Wi-Fi AP's. Several unmanaged switches are used to connect the many devices in the rooms. The Nighthawk submits the status information of all connected devices to HASS. This way I can track if everything is still online. Connected mobile phones are tracked for additional presence detection. 
  * [Unifi AP AC Lite](https://www.ui.com/unifi/unifi-ap-ac-lite/), the access points I use in the house to support full Wi-Fi coverage for the mobile and Wi-Fi connected devices.

* Voice control:
  * [Amazon Echo Dot 2nd gen.](https://amazon.com), Echo dot 2nd generation used for voice control. Currently this runs through HA Bridge, an emulated Hue application running on a RPi. Will be exchanged with HASS native Amazon Alexa support in the future.
  * [Google Home Mini](https://store.google.com/product/google_home_mini), currently not in use. But successfully tested with HASS.

* Media devices:
  * [LG Television](https://www.lg.com), my LG television running LG WebOS is controlled by HASS mainly for choosing the correct mode like Netflix or Spotify.
  * [Popcorn Hour](https://www.cloudmedia.com/), a classic in my house, the Popcorn A300. Mainly controlled with the Harmony Hub, but some initial settings are set by HASS at startup.
  * [Denon AV3808](https://www.denon.com), also a classic, but with network connect powerful for internet radio and streaming music, control over HASS in on the wish list.
  * [Apple TV](https://www.apple.com), not much used anymore these days as the LG can do most of it.
  * [Harmony Hub](https://www.logitech.com), the Harmony Hub, despite Logitech's poor way of supporting interoperability a powerful device for controlling the media devices. Integrated with HASS. V0.1 of the HASS control package is running in my house. V0.2 is in development.

## Homeassistant setup

### Homeassistant configuration
After looking at a ton of configurations and playing around with them I decided to go for packages. All my configurations are now in packages grouped as logical collections of components, entities automations etc.

For example package_notification contains the applied notification components (prowl and pushsafer currently), the associated entities for example the input_boolean that controls if notifications are enabled) and scripts. Scripts are used by many other packages that require  notifications.   

### Lovelace UI
I decided to fully go for Lovelace as the UI. As this is stable since the 0.87 release I moved everything over to Lovelace. When things are progressing I will upload some screenshots.
