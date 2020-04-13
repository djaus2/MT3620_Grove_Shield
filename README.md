# About Azure Sphere MT3620 Grove Shield Library

This is a library for the [Azure Sphere MT3620 Grove Shield](http://wiki.seeedstudio.com/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit). The shield enhances the [MT3620](http://wiki.seeedstudio.com/Azure_Sphere_MT3620_Development_Kit/) version of thye Azure Sphere by adding an I2C interface and Analog input. 
Original repository: [Azure Sphere MT3620 Grove Shield Library](https://github.com/Seeed-Studio/MT3620_Grove_Shield) on GitHub.

**Note:** _You now have to open Az Sphere projects as CMake projects, not as VS projects_

The following samples have been added/modified in this repository:
- Samples\Temp_and_Humidity_DME280
  - Reads the temperature, pressure and humidity via I2C using a DME280 connected to the Grove Azure Sphere MT3620 Shield
  - Nb: This shield implements its own I2C and communicates with the Az Sphere via its UART.
  - Nb2: The original driver in the repository only implements temperature. That functionality has been extended to include pressure and humidity.
  - I (carefully) used a BME280 that I had for other uses (non Grove) by carefully connected its pins to a Grove pin wire.
    - Black: Gnd
    - Red 3.3v 
    - White: SDA
    - Yellow: SCL
    - I also connected SD0 to 3.3v so that the I2C address is 0x77 <<1

 
## Requirements

- Operation Systems - Windows 10 or Ubuntu 18.04
- Developer Tools: Visual Studio [Visual Studio Community/Professional/Enterprise](https://visualstudio.microsoft.com/downloads/) or [Visual Studio Code](https://code.visualstudio.com/)
- Hardware - [Azure Sphere](https://www.seeedstudio.com/Azure-Sphere-MT3620-Development-Kit-US-Version-p-3052.html), [MT3620 Grove Shield](https://www.seeedstudio.com/MT3620-Grove-Shield-p-3145.html) 

## Create a MT3620 application

Review the [Azure Sphere documentation]https://docs.microsoft.com/en-au/azure-sphere/ for the guide to setting up the developer tools and Azure Sphere SDK on Windows 10 or Ubuntu 18.04.


## MT3620 Grove Shield App Manifest

Select __app_manifest.json__ in the application project, add the below attributions, so that we can use the peripherals that MT3620 Grove Shield would use.

```JSON
"Capabilities": {
    "Gpio": [ 8, 9, 10, 15, 16, 17, 18, 19, 20, 12, 13, 0, 1, 4, 5, 57, 58, 11, 14, 48 ],
    "Uart": [ "ISU0", "ISU3" ],
    "AllowedApplicationConnections": []
}
```

When using a hardware definition file, you must use an identifier for __app_manifest.json__.

```JSON
"Capabilities": {
    "Gpio": [ "$MT3620_GPIO8", "$MT3620_GPIO9", "$MT3620_GPIO10", "$MT3620_GPIO15", "$MT3620_GPIO16", "$MT3620_GPIO17", "$MT3620_GPIO18", "$MT3620_GPIO19", "$MT3620_GPIO20", "$MT3620_GPIO12", "$MT3620_GPIO13", "$MT3620_GPIO0", "$MT3620_GPIO1", "$MT3620_GPIO4", "$MT3620_GPIO5", "$MT3620_GPIO57", "$MT3620_GPIO58", "$MT3620_GPIO11", "$MT3620_GPIO14", "$MT3620_GPIO48" ],
    "Uart": [ "$MT3620_ISU0_UART", "$MT3620_ISU3_UART" ],
    "AllowedApplicationConnections": []
}
```

[Here](https://docs.microsoft.com/en-us/azure-sphere/app-development/manage-hardware-dependencies) for details of the hardware definition file.

## Some available header files

- Grove.h
- Sensors/Grove4DigitDisplay.h
- Sensors/GroveRelay.h
- <b> Sensors/GroveTempHumiBaroBME280.h </b> <-- This has been extended.
- Sensors/GroveTempHumiSHT31.h
- Sensors/GroveAD7992.h
- Sensors/GroveOledDisplay96x96.h
- Sensors/GroveRelay.h
- Sensors/GroveRotaryAngleSensor.h
- Sensors/GroveLEDButton.h
- Sensors/GroveLightSensor.h


## Usage of the library, see Example - Temp and Huminidy BME280

1. Add headers

```C
#include "Grove.h"
#include "Sensors/GroveTempHumiBaroBME280.h"
```

2. Initialize the shield in main() function

```C
int i2cFd;
GroveShield_Initialize(&i2cFd, 115200); // baudrate - 9600,14400,19200,115200,230400 
```

1. Initialize and instantiation

```C
void* bme280 = GroveTempHumiBaroBME280_Open(i2cFd);
```


4. Read temp and humidiy from the sensor
   
```C
    GroveTempHumiBaroBME280_ReadTemperature(bme280);
    GroveTempHumiBaroBME280_ReadPressure(bme280);
    GroveTempHumiBaroBME280_ReadHumidity(bme280);
    float temp = GroveTempHumiBaroBME280_GetTemperature(bme280);
    float humid = GroveTempHumiBaroBME280_GetHumidity(bme280);
    float press = GroveTempHumiBaroBME280_GetPressure(bme280)
```
