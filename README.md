# Home Assistant smart wireless doorbell

- [Home Assistant smart wireless doorbell](#home-assistant-smart-wireless-doorbell)
  - [Components](#components)
  - [Short story and motive](#short-story-and-motive)
  - [Tutorial](#tutorial)
    - [Setup development environment](#setup-development-environment)
    - [Connect the ESP32 board with the RF Receiver](#connect-the-esp32-board-with-the-rf-receiver)
    - [Determine the payload of the button](#determine-the-payload-of-the-button)
    - [Configure your ESP32 to listen for doorbell rings](#configure-your-esp32-to-listen-for-doorbell-rings)
    - [Create a sensor in Home Assistant](#create-a-sensor-in-home-assistant)

## Components

1. Wireless doorbell using the 433MHz frequency
2. ESP32 board
3. RF Receiver for 433MHz
4. Home Assistant
5. MQTT broker

The exact components that I use are in the links below, but the process and code should is the same.

## Short story and motive

I want to have a smart doorbell in order to receive notifications on my phone/watch when someone rings it, but it's not legally allowed record public areas in my country of residence, that means that I can't place a smart doorbell like the Eufy Doorbell, or Ring, because all of them come with integrated cameras. So I embarked on a quest to use a regular wireless doorbell (as I don't have any wiring there) and make it smart.

## Tutorial

In this tutorial I'll explain the steps and configuration that I've setup. If you choose to use a different board, or a different pin, then some of the steps will need to be modified based on your use-case.

### Setup development environment

1. Download and install [Arduino IDE](https://www.arduino.cc/en/software)
2. Go to *Tools -> Manage Librarires... (Ctrl + Shift + I)*
3. Install [ArduinoMqttClient](https://github.com/arduino-libraries/ArduinoMqttClient)
4. Install [rc-switch](https://github.com/sui77/rc-switch)
5. Go to *Tools -> Board*: and set the type of your board. In my case it is **ESP32 Dev Module**

### Connect the ESP32 board with the RF Receiver

![Connection chart](https://github.com/vasililiev/esp32-433-receiver-home-assistant/blob/master/img/connection_chart.png)

### Determine the payload of the button
Every wireless doorbell comes pre-configured to send a specific payload over the 433MHz frequency and what you first need to do is to decode the payload that's transmited by the doorbell button.

Use the [code for the ESP32 board](https://github.com/vasililiev/esp32-433-receiver-home-assistant/blob/master/01_determine_payload/detect_rf433_payload.inno) to determine the payload that your wireless button transmits on the 433Mhz frequency.

### Configure your ESP32 to listen for doorbell rings
After you discover the payload, copy the [code for the ESP32 board](https://github.com/vasililiev/esp32-433-receiver-home-assistant/blob/master/02_receive_signals_and_publish_to_ha/esp32_rf_mqtt_doorbell.inno), fill in the blanks, and upload it to your ESP32.

### Create a sensor in Home Assistant

Add the [binary sensor and Home Assistant restart automation](https://github.com/vasililiev/esp32-433-receiver-home-assistant/blob/master/03_expose_sensor_in_ha/ha_config.yaml) in your configuration and in your MQTT sensors accordingly.

**Note:** The Home Assistant automation is neccesery in order for the sensor to reliably work. Without the automation, each time Home Assistant restarts the MQTT connection will be lost in the ESP32 and no updates will be published.