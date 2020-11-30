# hass-notes
Notes for deploying Home assistant

## Set Up
This guide assumes you're running on the following:

- Ubuntu 20.04

### Installing Docker

### Installing KVM

### Setting up the KVM Bridge

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
