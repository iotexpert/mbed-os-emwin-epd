# mbed-os-emwin-epd
This repository contains the configuration files required to run emWin using the CY8CKIT-028-EPD shield.  That shield has a Pervasive EPD Display that is attached using a SPI Bus plus a set of control GPIOs.

These files contain the Segger GUI configuration files as well as the the EPD interface files.  The interface is implemented using the mbedos HAL.

To use these files you should
* mbed add git@github.com:iotexpert/mbed-os-emwin-epd.git
* mbed add git@github.com:cypresssemiconductorco/emwin.git

The pins are correct for the targets
* CY8CKIT062_BT
* CY8CKIT062_WIFI_BT

To use this library you need to add the emWin library to mbed_app.json
```json
"target_overrides": {
        "*": {
            "target.components_add": ["EMWIN_OSNTS"]
        }
```
This implementation uses the Segger BitPlains LCD driver which just writes data into an SRAM FrameBuffer.  The application is then responsible for dumping the Frame Buffer to the screen.  This can be done by calling
```
Cy_EINK_UpdateDisplay(LCD_GetDisplayBuffer(),CY_EINK_FULL_4STAGE, true);
```
Here is an example main.cpp
```
#include "mbed.h"
#include "GUI.h"
#include "cy_cy8ckit_028_epd.h"
#include "LCDConf.h"

int main()
{
    GUI_Init();
 
    GUI_SetColor(GUI_BLACK);
    GUI_SetBkColor(GUI_WHITE);
    GUI_Clear();
    GUI_SetFont(GUI_FONT_32B_1);
    GUI_SetTextAlign(GUI_TA_CENTER);
    GUI_DispStringAt("Hello World", GUI_GetScreenSizeX()/2,GUI_GetScreenSizeY()/2 - 16);

    Cy_EINK_UpdateDisplay(LCD_GetDisplayBuffer(),CY_EINK_FULL_4STAGE, true);
}
```
If you need to use it for a different kit you can add pins using the target overides like this:
```json
"target_overrides": {
        "*": {
            "target.components_add": ["EMWIN_OSNTS"]
        },
        "CY8CKIT_062_WIFI_BT" : {
            "EPD_MOSI":"P12_0"
        }
    }
```
All of the pins are defined in mbed_lib.json
{
    "name" : "EPD",
    "config": {
            "MOSI":        "P12_0",
            "MISO":        "P12_1",
            "SCLK":        "P12_2",
            "DISPCS":      "P12_3",
            "DISPRST":     "P5_2",
            "DISPBUSY":    "P5_3",
            "DISCHARGE":   "P5_5",
            "DISPEN":      "P5_4",
            "BORDER":      "P5_6",
            "DISPIOEN":    "P0_2"
    }
}
```
