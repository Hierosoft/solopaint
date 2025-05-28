# sm-arter
STM32 embedded display drawing tablet


## Development
Planning document: https://docs.google.com/document/d/1PIKT0VAlkEhiWQb-OlrnNMqQhXV-2bqdCYlou2-hG1E/edit?usp=sharing

### Requirements
- [STM32CubeIDE](https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-ides/stm32cubeide.html)
  - Documentation: In the Welcome Screen, click "STM32CubeIDE manuals"
    (on your computer, such as <file:///home/owner/st/stm32cubeide_1.18.1/plugins/com.st.stm32cube.ide.documentation_2.3.101.202504042321/docs/DM00603738.pdf>)
- [TouchGFX Designer](https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-configurators-and-code-generators/touchgfxdesigner.html#overview) (Windows only)
  - Get prerequisites: https://support.touchgfx.com/docs/introduction/prerequisites
    - NOTE: Requires Windows (for GUI design, not for compiling the generated code) according to the getting prerequisites page)
  - Install the zip into STM32CubeIDE (https://support.touchgfx.com/docs/introduction/installation, but steps below are fixed):
    - Help, Configuration Tool, Manage embedded software packages
    - Refresh
    - STMicroelectronics tab
    - Expand the "X-CUBE-TOUCHGFX" category (click "+")
      - Check "TouchGFX Generator"
      - Click "Install Now"
    - Install STM32CubeProgrammer (and STM32 ST-LINK Utility for Windows if necessary): <https://support.touchgfx.com/docs/introduction/installation#installing-stm32cubeprogrammer>
  - [Documentation](https://www.st.com/en/development-tools/touchgfxdesigner.html#documentation)

### Settings used
#### STM32CubeMX (UNUSED, see STM32CubedIDE above instead)
TrustZone (<https://wiki.stmicroelectronics.cn/stm32mcu/wiki/Security:How_to_disable_TrustZone_in_STM32L5xx_devices_during_development_phase>) is turned off since any server connections are done at the user level.

Code Generator, HAL Settings, "Set all free pins as analog (to optimize power consumption)" is enabled, since the goal is to make a battery-powered device.

Pinout Configuration, System Core, ICACHE is enabled (as recommended by the IDE):
- "2-ways set associative cache" enabled: Reduces cache misses
- "Memory address remap" enabled: lets the microcontroller rearrange the memory map to optimize access patterns

SMPS is enabled (as recommended by the IDE):
- Pinout Configuration, Power and Thermal
  - PWR, Low Power, Power saving mode (enabled)
    - Configuration, System power supply
      - Power regulator: "SMPS"


## Hardware
- Microcontroller: STM32U5G9NJH6Q (4MB Flash, 3MB RAM)
- Resolution: 800 x 480
- The Chrom-ART Accelerator (DMA2D), should probably be used for alpha blending (Neo-Chrom VG processor is vector-based and could be used as a workaround but that would be extra steps).
- CTP driver (Capacitive Touch Panel controller): ILI2132A
- Supply voltage for module: 6.0 - 48.0 V
- Brightness: 850 cd/m^2
- Poikilos has the air bonding model (above). JeffDarla has the higher quality optical bonding model provided by Poikilos. The main difference is the visual appearance (and possibly capacitance responsiveness or other characteristics).
- Everything on the datasheet etc other than the external characteristics applies to both models.

Product page (RVT50HQSNWC01): https://riverdi.com/product/5-inch-tft-lcd-screen-stm32u5-embedded-display-capacitive-touch-panel-air-bonding-rvt50hqsnwc01

The product page includes a TouchGFX "getting started" link, but there is a more specific getting started page for the embedded display elsewhere: https://riverdi.com/blog/how-to-get-started-with-riverdi-stm32-u5-embedded-display-tutorial-by-controllerstech