# raspi-play
Build a low-profile AirPlay 2 audio receiver, powered by Raspberry Pi! This is a cheap and easy project to wirelessly connect an old speaker system to your Apple devices, such as an Apple HomePod.

![front of device (audio and SD ports)](./images/IMG_8580.jpeg?raw=true)
![back of device (power and data ports)](./images/IMG_8579.jpeg?raw=true)

### Note
This tutorial assumes you have a Unix-like terminal for setting up the Raspberry Pi

## Materials
- Raspberry Pi Zero 2 W - [Amazon](https://a.co/d/fDa0be4) | [Adafruit](https://www.adafruit.com/product/5291)
- I2S PCM5102A DAC Decoder - [Amazon](https://a.co/d/4PBEBoA) | [Ebay](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p4432023.m570.l1313&_nkw=QCCAN+Interface+I2S+PCM5102A+DAC+Decoder+GY-PCM5102+I2S+Player+Module+pHAT+Format+Board+Digital+PCM5102+Audio+Board+for+Raspberry+Pi&_sacat=0)
- MicroSD Card (8GB+) - [Amazon](https://a.co/d/fdQJwdG) | [Western Digital](www.westerndigital.com/products/memory-cards/sandisk-ultra-uhs-i-microsd)
- USB or SD card adapter for the MicroSD card if your computer does not have a MicroSD slot (your MicroSD card may come with one, read its description) - Easily found on Amazon
- 3D printed case (use the case and cover STL files in the repository [coming soon])

## Tools
- 3D Printer
- Soldering Iron
- Solder (preferably rosin core, lead-free)
- Jumper wires (if your DAC decoder does not come with them, mine did)
- Third hand tool/PCB holder (optional but recommended)
- Hot glue & glue gun
- Metal file (or something else to shave down the HDMI port's overhang)

## Instructions

### 1. Gather the parts specified

### 2. Prepare the Pi to fit into the 3D-printed case

Discard the black piece of plastic covering the ribbon cable connector on the short edge of the Pi.

File away the metal on the HDMI port that overhangs the edge of the PCB board (notice in the second image how the HDMI port cannot be seen from underneath).

![Pi preparation1](./images/IMG_8657.jpeg?raw=true)
![Pi preparation2](./images/IMG_8658.jpeg?raw=true)

### 3. Solder jumper wires to the DAC decoder

On the short side of the board, the pads `BCK`, `DIN`, `LCK/LRCK`, `GND`, and `VIN` need to have jumper wires soldered to them. Do NOT solder a wire to `SCK`.

Apply a small bead of solder to jump the two little pads on the front side of the board, between `SCK` and `BCK`. **NOT THE `SCK` AND `BCK` PADS THEMSELVES, but the tiny pads next to them.** (See image below)

![DAC connections](./images/IMG_8656.jpeg?raw=true)

### 4. Solder the jumper wires from the DAC to the Pi

Make sure the wires go into the front component side of the Pi, otherwise the Pi will not fit into the case properly. Please use the following guide:

**Connection guide**
```
DAC BOARD   > Raspberry Pi Zero 2 W connector
-----------------------------------------------
SCK         > Not wired (Internally generated)
BCK         > PIN 12    (GPIO18)
DIN         > PIN 40    (GPIO21)
LCK/LRCK    > PIN 35    (GPIO19)
GND         > PIN 6     (GND) Ground
VIN         > PIN 2     (5V)
```
**GPIO labels**
![GPIO guide](./images/IMG_8659.jpeg?raw=true)

This is how mine looked after this step:

![Pi connections](./images/IMG_8662.jpeg?raw=true)

### 5. Flash the SD card

On your computer, plug the MicroSD card into your MicroSD reader slot or adapter.

Download the Raspberry Pi imager from the [Raspberry Pi software page](https://www.raspberrypi.com/software/).

Launch the application and grant the necessary permissions. You should see a screen for selecting a device, an operating system, and storage.

For the device, choose `RASPBERRY PI ZERO 2 W`. The operating system should be `RASPBERRY PI OS (LEGACY, 64-BIT) LITE`.

![Raspberry Pi flash software](./images/IMG_8678.jpeg?raw=true)

Select your MicroSD card or slot name from the Storage options, mine was called "APPLE SDXC READER MEDIA".

Click "NEXT". If it asks you if you want to configure any settings before it flashes the MicroSD card, I recommend setting a custom hostname, username and password.

*The hostname is what will show up by default when connecting to the device over AirPlay, I named mine "AirPlayRBP", but if you leave it as default, it will be called "raspberrypi"*

### 6. Configure the network settings

Navigate to the root directory of the SD card's boot partition, `bootfs`.

Create a new file there called `wpa_supplicant.conf` using your preferred text editor. Open the file and paste in the template below:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<YOUR COUNTRY CODE>

network={
    ssid="<YOUR 1ST NETWORK NAME>"
    psk="<YOUR 1ST NETWORK PASSWORD>"
    key_mgmt=WPA-PSK
    id_str="<YOUR 1ST NETWORK ID>"
    priority=<YOUR 1ST NETWORK PRIORITY>
}

network={
    ssid="<YOUR 2ND NETWORK NAME>"
    psk="<YOUR 2ND NETWORK PASSWORD>"
    key_mgmt=WPA-PSK
    id_str="<YOUR 2ND NETWORK ID>"
    priority=<YOUR 2ND NETWORK PRIORITY>
}
```

Fill in your personalized preferences for each WiFi network you wish to add (anything with angular brackets `<...>`)

- `country`: the two-letter country code (i.e. `country=US`, `country=AU`, etc.)
- `ssid`: the case-sensitive name of the WiFi network (surrounded by double quotes)
- `psk`: the password (surrounded by double quotes
- `id_str`: a unique label to distinguish from other networks, could be anything (surrounded by double quotes)
- `priority`: a unique number to determine what order WiFi networks should connect in (integer, no quotes)

This configuration is for two WiFi networks, but a different number of networks can be configured by adding or removing `network={...}` listings

Save and close `wpa_supplicant.conf`

Create another file called `ssh` with no file extension

In a Unix-like terminal, you can do this with the following command

```sh
touch ssh
```


Verify that the Pi is connected to your network with the following command, assuming you are using the default hostname:
```sh
ping raspberrypi.local
```
To open the Pi's terminal, you can SSH into it. The following command assumes you are using the default credentials:
```sh
ssh pi@raspberrypi.local
```
The first time you SSH into the Pi, it will ask you if you want to continue to connect. Type `yes`. It will then prompt you for a password. The default password is `raspberry`.

Next, run these commands one at a time:
```sh
sudo apt-get update
sudo apt-get upgrade
curl -sSL get.pimoroni.com/phatdac | bash
sudo apt-get install git
sudo apt-get install autoconf
```

Follow the instructions to install the following:
- [NQPTP](https://github.com/mikebrady/nqptp)
- [Shairport Sync](https://github.com/mikebrady/shairport-sync/blob/master/BUILD.md)

Note: Whenever a command starts with a `#` symbol instead of a `$`, it means to run the command with root privileges, i.e. starting with `sudo ...`.

[to be continued]

## Resources
- Code to run AirPlay server: [Shairport Sync](https://github.com/mikebrady/shairport-sync)
- DAC jumper wire connections: [Kamran Sethi](https://raspberrypi.stackexchange.com/a/76264)
