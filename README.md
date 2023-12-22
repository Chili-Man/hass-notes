# hass-notes
Notes for deploying Home assistant

## Set Up
This guide assumes you're running on the following:

- RaspberryPi 4 (with at least 8GB RAM, 64 GB MicroSD card Application Class 2)
- [Home Assistant Operating System](https://github.com/home-assistant/operating-system)
- Z-Wave controller *(

### Installing Home Assistant Operating System
The refernce steps are available at: https://www.home-assistant.io/getting-started/

1. Use Balena Etcher tool to brun the HassOS to the SD card: https://github.com/balena-io/etcher/releases

2. Download the latest stable 64-bit version of HassOS from (`hassos_rpi4-64-*`): https://github.com/home-assistant/operating-system/releases


### Initial Boot
After installing the freshly minted HassOS SD card to the RaspberryPi, turn it on.

1. You will want to give a static IP address on your router.
2. Login to the router to find out what IP address was given to HomeAssistant or go to http://homeassistant.local:8123/ if the router supports mDNS
3. Fill out the initial information

### Open Z-Wave Set Up

2. Install the OpenZWave add-on.
3. Go to the OpenZWave add-on configuration page.
    - For the `device` option set it to the USB device id, such as `/dev/serial/by-id/usb-0658_0200-if00`. You may find the correct value for this on the `Supervisor -> System -> Host system -> Hardware page`
    - For the network key, generate it with `cat /dev/urandom | tr -dc '0-9A-F' | fold -w 32 | head -n 1 | sed -e 's/\(..\)/0x\1, /g' -e 's/, $//'`. Save this key somewhere securely and safely
    - Under the `Newtork` section, map port `1983` -> `1983` on the Host so that we can remotely access the `ozw-admin`
4. Start the OpenZWave add on.
5. You can either configure the Z-Wave controller through the add-on provided ingress web-ui; or you can also install the [ozw-admin](https://github.com/OpenZWave/ozw-admin/releases) and connect to it remotely

**Note:** From the OZW-Admin gui, the Z-Wave Plus capability will be
unchecked even if the controller device does have support for it. Apparently,
that behavior is expected.


## Adding Z-Wave Devices
Apparently, adding to many secure devices to a Z-Wave network can cause problems.
There are devices that don't need to be added securely such as temperature and
humiidty sensors; i.e. things only output events. Devices that can recieve commands
for automation purposes such as triggering an alarm.

### Unsecure
To add a new Z-Wave device (unsecure), also known as a `node`:

1. Go to the `Configuration` page
2. Then click on `Integrations`, which will bring you to the integration overview page
3. Click on the `OpenZWave` `configure` button

### Secure

## Debugging
### SSH Access to the Host
See https://developers.home-assistant.io/docs/operating-system/debugging/ .

This will allow you to directly access the underlying host, which means you
should be able to directly access the logs of the running Docker containers

### Installing MQTT (Mosquitto)
Deploy the official container image:

`docker run -d -p 1883:1883 eclipse-mosquitto:1.6.12`


#### Receive Test Message
To send a test message:

`docker run --rm -i -t eclipse-mosquitto:1.6.12 mosquitto_sub -h "${MQTT_HOST}" -p 1883 --topic yeet`

See https://mosquitto.org/man/mosquitto_sub-1.html for more details.


#### Publish Test Message
To send a test message:

`docker run --rm -i -t eclipse-mosquitto:1.6.12 mosquitto_pub -h "${MQTT_HOST}" -p 1883 --topic yeet --message 'hello'`

See https://mosquitto.org/man/mosquitto_pub-1.html for more details
