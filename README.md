# DIY Air Quality Monitor

This project contains the software for a DIY air quality monitor based on the Panasonic SN-GCJA5 air quality sensor. This software is designed to run on a [TinyPICO](https://www.tinypico.com) ESP32 development board and be built by PlatformIO in Visual Code Studio.

To use this code, first edit `include/Configuration.h` as needed, then build and upload to the TinyPICO. The Panasonic SN-GCJA5 is connected to the TinyPICO via it's serial connection at pin 33.

*Currently this project very much a work in progress.*

## Reference Material

* [Panasonic SN-GCJA5 Product Specification](https://na.industrial.panasonic.com/products/sensors/air-quality-gas-flow-sensors/lineup/laser-type-pm-sensor/series/123557/model/123559)
* [Panasonic SN-GCJA5 Communications Specification](https://b2b-api.panasonic.eu/file_stream/pids/fileversion/8814)

## Bill of Materials
To build this project, you will need the folloing components:

* [Panasonic SN-GCJA5 Sensor](https://www.mouser.com/ProductDetail/667-SN-GCJA5) - This is the particulate counter that the project is centered around.
* [GHR-05V-S Cable Housing](https://www.digikey.com/en/products/detail/jst-sales-america-inc/AGHGH28K305/6009450) - You need to make a cable to connect the SN-GCJA5. This is the connector housing fo rthe end of the cable that connects to the sensor. You are free to determine how you connect the other end of the cable to the microntroller.
* [Pre-crimped Jumper Wire](https://www.digikey.com/en/products/detail/jst-sales-america-inc/AGHGH28K305/6009450) - These are pre-made cables that have the special connector for the SN-GCJA5 crimped onto both ends. You only need the connector on one end, and that end gets inserted into the cable housing from the previous line. At a minimum you need three of these, for the +5V, GND, and TX connection on the sensor. If you wish to make a cable that also enables the I2C connection to the sensor (even though this project does not use it), get 5 to fully pupulate the cable housing. See the SN-GCJA5 Product Specification for information on placement of each cable with respect to the connector n the sensor.
* ESP32 Microcontroller Board - This code is written to support various ESP32 microntorller development boards:
  * [TinyPICO ESP32 Microcontroller Board](https://unexpectedmaker.com/shop/tinypico) - The TinyPICO is very versatile, has a good WiFi antenna, has PSRAM already installed (which this project uses), and the form factor can't be beat. The TinyPICO is the preferred ESP32 board for this project.
  * [EzSBC ESP32 Development Board](https://www.ezsbc.com/product/esp32-breakout-and-development-board/) - Another fine ESP32 development board. However, this EzSBC board does not come with PSRAM installed, which limits the historical data that can be retained by the microcontroller.
* [Dupont Cable Housing](https://www.ebay.com/itm/100Pcs-1P-Dupont-Jumper-Wire-Cable-Housing-Female-Pin-Connector-2-54-mm-Pitch/112299848779) - My prefered way of connectng the sensor to the TinyPICO is to use female dupont connectors to the pins that get soldered to the TinyPICO. Here are the housings to make those connectors on the other end of the cable you need to build to connect the SN-GCJA5.
* [Female Pin Dupont Connector](https://www.ebay.com/itm/US-Stock-100pcs-Female-Pin-Dupont-Connector-Gold-Plated-2-54mm-Pitch/371912445248) - My prefered way of connectng the sensor to the TinyPICO is to use femal dupont connectors to the pins that get soldered to the TinyPICO. Here are the female connectors needed to make the connectors.
* *OPTIONAL* [BME680 Environment Sensor Board](https://www.digikey.com/products/en?mpart=3660&v=1528) - You can optionally attach a BME680 sensor to this project to additionally get temperature, pressure, and humidity measurements along with the air quality measurement that the SN-GCJA5 provides. Note that you will need some 26 to 30 AWG hook up wire to construct the cables needed to connect the BME680 to to the TinyPICO, but you can use on either end of those cables the dupont connectors that you are ordering SN-GCJA5.

## Pin Connections

### Panasonic SN-GCJA5
| ESP32 Pin | Sesnor Pin | Description |
|:-:|:-:|:--|
| `5V` | 5 | The Panasonic SN-GCJA5 uses 5V power. On most ESP32 boards this is marked as either `5V` or `Vusb`. |
| `GND` | 4 | Ground |
| `IO33` | 1 | The Panasonic SN-GCJA5 serial TX line (so RX on the ESP32). Note that this serial line operates at 3.3V, so it is voltage safe for the ESP32 |

### BME680 Environment Sensor Board
| ESP32 Pin | Sesnor Pin | Description |
|:-:|:-:|:--|
| `3v3` | `Vcc` | The BME680 runs off of 3.3V. |
| `GND` | `GND` | Ground |
| `IO22` | `SCL` | The I2C clock line |
| `IO21` | `SDA` | The I2C data line |

## TODO
The following features are planned. Listed in no particular order.

1. Allow different rates for data collection from sensor and data posting to telemetry service. Note that the data collection rate must be greater than or equal to the telemetry posting rate.
2. ~~Give options for look-back window of average AQI in the web UI~~
   * Add cookie to remember user's last selected averaging window. 
3. ~~Add support for the BME680 sensor, which would give gas, pressure, temperature & humidity readings.~~
   * Make the units used for the display of the temperature (celsius or fahrenheit) configurable
4. Add support for an ePaper display that does the following:
   * Display the average AQI (configurable look back window)
   * Display a warning based on the color code of the AQI
   * Display the BME 680 sensor data, if attached.
   * Display the web UI URL
5. Create a web UI to set up and configure the monitor, replacing the `Configuration.h` file. Would depend on ePaper display to display the temporary WiFi AP the user needs to connect to to configure.
6. Add ability to download history as a CSV from web UI.
