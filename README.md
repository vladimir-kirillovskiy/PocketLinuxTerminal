# Pocket Linux Terminal


## Headless set up

I mostly followed this guide
https://www.instructables.com/The-Ultimate-Headless-RPi-Zero-Setup-for-Beginners/

Main points are:
1. Install the RNDIS driver for Windows
2. in config.txt
``` dtoverlay=dwc2 ``` 
3. in cmdline.txt
``` modules-load=dwc2,g_ether ```

* Reboot a few times to update "cmdline.txt". 

After that, you can sharethe  internet from Windows

## Screen set up
Used it as a reference, but with some changes
https://github.com/adamomd/ILI9488RPIScript


Connected it like that
| Screen Pin | Pi Physical Pin | BCM GPIO	Purpose | |
|----------|----------|----------|----------|
| VCC |  Pin 1 |  3.3V | Power (VCC) |
| GND |  Pin 6 |  GND | Ground |
| CS |  Pin 24 | GPIO 8 | Chip Select (SPI) |
| RST |  Pin 22 |  GPIO 25 | RESET |
| DC | Pin 18  | GPIO 24  | Data/Command (SPI) |
| MOSI | Pin 19  | GPIO 10  | MOSI |
| SCK |  Pin 23 | GPIO 11  | Clock |
| LED |  Pin 12 or 3.3V | GPIO 18 or any 3.3V | Backlight |
| MISO |  Pin 21 | GPIO 13 | MISO |


sudo raspi-config
Enable SPI and expand the filesystem

config.txt
```
dtparam=spi=on
hdmi_force_hotplug=1
framebuffer_width=320
framebuffer_height=480
```

sudo apt update
sudo apt install -y git cmake build-essential vim tmux

git clone https://github.com/juj/fbcp-ili9341.git
```
cd fbcp-ili9341
mkdir build
cd build
```

Used one of those, don't remember which one, I think both should work
```
sudo cmake   -DILI9488=ON   -DSPI_BUS_CLOCK_DIVISOR=6   -DGPIO_TFT_DATA_CONTROL=24   -DGPIO_TFT_RESET_PIN=25  -DSINGLE_CORE_BOARD=ON -DSTATISTICS=0  ..
sudo cmake   -DILI9488=ON   -DSPI_BUS_CLOCK_DIVISOR=4   -DGPIO_TFT_DATA_CONTROL=24   -DGPIO_TFT_RESET_PIN=25  -DSINGLE_CORE_BOARD=ON -DSTATISTICS=0 -DUSE_DMA_TRANSFERS=ON  ..
```

Can add this parameter if needed to rotate the screen
``` -DDISPLAY_ROTATE_180_DEGREES=ON ```

```
sudo make -j
sudo ./fbcp-ili9341 &
```
Should work at this point

## Keyboard

First enable i2c in raspi-config 

Then use this repository
```https://github.com/ian-antking/cardkb?tab=readme-ov-file```


