# raspi-play
Build a low-profile AirPlay audio receiver, powered by Raspberry Pi! This is a cheap and easy project to wirelessly connect an old speaker system to your Apple devices, such as an Apple HomePod.

![front of device (audio and SD ports)](./images/IMG_8580.jpeg?raw=true)
![back of device (power and data ports)](./images/IMG_8579.jpeg?raw=true)


## Materials
- Raspberry Pi Zero 2 W - [Amazon](https://a.co/d/fDa0be4) | [Adafruit](https://www.adafruit.com/product/5291)
- I2S PCM5102A DAC Decoder - [Amazon](https://a.co/d/4PBEBoA) | [Ebay](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p4432023.m570.l1313&_nkw=QCCAN+Interface+I2S+PCM5102A+DAC+Decoder+GY-PCM5102+I2S+Player+Module+pHAT+Format+Board+Digital+PCM5102+Audio+Board+for+Raspberry+Pi&_sacat=0)
- MicroSD Card (8GB+) - [Amazon](https://a.co/d/fdQJwdG) | [Western Digital](www.westerndigital.com/products/memory-cards/sandisk-ultra-uhs-i-microsd)
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
1. Gather/print the parts specified.
1. We will start by soldering jumper wires to the DAC decoder. Take a look at the five connectors on the short edge of the board. We will be soldering our jumper wires to four of them -- `BCK`, `DIN`, `LCK/LRCK`, `GND`, and `VIN`. Don't solder any wire to `SCK`, we will instead apply a small bead of solder to jump the two little pads on the front side of the board, between `SCK` and `BCK` (NOT THE `SCK` AND `BCK` PADS THEMSELVES, but the tiny pads near them).

![DAC connections](./images/IMG_8656.jpeg?raw=true)

3. Before we connect these to the Raspberry Pi, we need to prepare the Pi to fit into the 3D-printed case. Discard the black piece of plastic covering the ribbon cable connector on the short edge of the Pi. Use the metal file to grind away the metal on the HDMI port that overhangs the edge of the PCB board.

![Pi preparation1](./images/IMG_8657.jpeg?raw=true)
![Pi preparation2](./images/IMG_8658.jpeg?raw=true)

5. Now we can solder the jumper wires from the DAC to the Pi. When you solder the jumper wires, make sure the wires come out of the front component side of the Pi, otherwise the case will not fit. Here is a connection guide:

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

![GPIO guide](./images/IMG_8659.jpeg?raw=true)

6. 
`bootfs/wpa_supplicant.conf`
```conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<YOUR TWO LETTER COUNTRY CODE i.e. US>

network={
    ssid="<YOUR 1ST NETWORK NAME>"
    psk="<YOUR NETWORK PASSWORD>"
    key_mgmt=WPA-PSK
    id_str="<YOUR CUSTOM LABEL>"
    priority=1
}

network={
    ssid="<YOUR 2ND NETWORK NAME>"
    psk="<YOUR NETWORK PASSWORD>"
    key_mgmt=WPA-PSK
    id_str="<YOUR CUSTOM LABEL>"
    priority=2
}

...
```

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
