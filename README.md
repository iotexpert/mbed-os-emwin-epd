# mbed-os-emwin-epd
This repository contains the configuration files required to run emWin using the CY8CKIT-028-EPD shield.  That shield has a Pervasive EPD Display that is attached using an SPI Bus plus a set of control GPIOs

These files contain the GUI configuration files as well as the the EPD interface files.  The interface is implemented using the mbedos HAL.

To use these files you should
* mbed add git@github.com:iotexpert/mbed-os-emwin-epd.git


The pins are correct for the targets
* CY8CKIT062_WIFI_BT

To use this library you need to add the emWin library to mbed_app.json
```json
"target_overrides": {
        "*": {
            "target.components_add": ["EMWIN_OSNTS"]
        }
```
If you need to use it for a different kit you can add pins using the target overides like this:
```json
"target_overrides": {
        "*": {
            "target.components_add": ["EMWIN_OSNTS"]
        },
        "CY8CKIT_062_WIFI_BT" : {
            "ST7789V_TFT.TFTRD":"P12_3"
        }
    }
```
All of the pins are defined in mbed_lib.json
```json
{

}
```
