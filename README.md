# PIC 16 - Azure IoT Embedded C SDK PnP Sample Code

This page shows the steps needed to get started with the [Microchip PIC 16](https://www.microchip.com/developmenttools/ProductDetails/AC164164) board and get it to work with the [Azure IoT Embedded C SDK](https://github.com/Azure/azure-sdk-for-c/tree/master/sdk/iot).

## Prerequisites

Before proceeding, you should have the following ready:

- [Setup your Azure IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
- [Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases)
- Download the PnP DTMI [here](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json)
- [MPLAB X IDE V5.30 or later](https://www.microchip.com/mplab/mplab-x-ide)
- [XC16 Compiler v1.50 or later](https://www.microchip.com/mplab/compilers)
- MPLAB code configurator (Once you finish the installation of the previous items, open MPLAB X IDE, then click Tools > Plugin Download > search for MPLAB code configurator and install it)

![code-configurator](docs/Images/code_configurator.png)  

## Sample Instructions

### Get the files

1. Get the sample files from GitHub: [Link to Eric's GitHub repo](https://garage-06.visualstudio.com/MicrochipIoT%202027%2088114/_git/PIC_IoT?version=GBmaster)

2. Open the project in MPLAB X: File > Open Project > [select the folder in which you just downloaded the files]. The MPLAB X project name is "myiot"

![project](docs/Images/project.png)

### Get the Device ID

The device_ID on both IoT hub and the device itself must match, so before creating a new device in Iot Hub, you must retrieve your device_ID. (If you're using DPS - device Provisioning Service, you don't need to worry with this step.)

- You can find the device_id on the IoT_Sensor_Node_config.h file.
- To find it, on the left hand panel, look for Header files > platform > config > IoT_Sensor_Node_config.h
- Line 39 shows the ``#define DEVICE_ID "your_device_number"`

![sensor node](docs/Images/sensor_node.png)

Create a new device on your IoT Hub using this device as your device name.

![device_id](docs/Images/device_id.png)  

### Update your credentials

1. Update the Wi-Fi credentials and security

    - On the left hand panel, look for Header files > platform > config > config_winc.h

    ![winc](docs/Images/conf_winc.png)

    - Update your wi-fi SSID name: Line 25 shows `#define CFG_MAIN_WLAN_SSID "your-ssid"`
    - Update your wi-fi Password:  Line 42 shows `#define CFG_MAIN_WLAN_PSK "your-ssid-password"`
    - Update your wi-fi security: Line 36 shows `#define CFG_MAIN_WLAN_AUTH M2M_WIFI_SEC_WPA_PSK` (most home routers use M2M_WIFI_SEC_WPA_PSK, If this is you case, there's no need for updating this line)

2. Update the device_key and MQTT_Host

    - On the left hand panel, look for Header files > platform > config > IoT_Sensor_Node_config.h
    - Line 44 shows the `DEVICE_KEY`: replace it by the Primary Key you find at your device page on Azure Portal.
    - Still on this file, look at line 23 and update the `CFG_MQTT_HOST` information.
    - You'll need your device's primary connection string for that.

    Note: you don't need the whole connection string, you only need the host name. In the example connection string below, the highlighted is the part you need to update on line 23.

```c
HostName=**ps-demo-hub.azure-devices.net**;DeviceId=Pic_test_2;SharedAccessKey=PaPq+m4gtXvYs=
```

### Upload the Sample to the Device

1. Once you have all the credentials set, you can upload the code to your device, by clicking on debug project.

    ![debug_button](docs/Images/debug_button.png)

2. If everything went well, you should see the onboard LEDs turning on. The blue and the green LEDs will be on all the time and the yellow one blinks every 5 seconds to confirm the telemetry has been sent to Azure.

### Interacting with Your PnP Device

Once the board is flashed, you can proceed to interact with the PnP device using the IoT Explorer. The following steps make it as seamless as possible.

- Once you open the tool, on the left side, there should be a tab called "IoT Plug and Play Settings". Open that tab and add the directory where you downloaded the DTMI detailed from [prerequisites](#prerequisites)
- Paste in the connection string for your IoT Hub from the portal.
- Find your device listed after clicking on your newly connected IoT Hub and navigate to the left hand side where you will find a "IoT Plug and Play components" tab.
- There, click on the component listed near the bottom and use the tabs that result on the top to use the various features (Commands, Twin, Telemetry, etc). It should look similar to the picture below.

    ![Iot Explorer](./docs/Images/iotexplorer.png)
