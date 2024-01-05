<div align="center"><h1>raspi-play</h1></div>
<div align="center">Build a low-profile <b>AirPlay 2</b> audio receiver, powered by Raspberry Pi! This is a cheap and easy project to wirelessly connect an old speaker system to your Apple devices, such as an Apple HomePod.</div><br/>

<div align="center">
  <img src="./images/IMG_8580.jpeg?raw=true" alt="front of device (audio and SD ports)" width="47%" />
  <img src="./images/IMG_8579.jpeg?raw=true" alt="back of device (power and data ports)" width="47%" />
</div><br/>

<div align="center"><i>This tutorial assumes you have a Unix-like terminal for setting up the Raspberry Pi</i></div>

<div align="center"><h2>Materials</h2></div>

- Raspberry Pi Zero 2 W - [Amazon](https://a.co/d/fDa0be4) | [Adafruit](https://www.adafruit.com/product/5291)
- I2S PCM5102A DAC Decoder - [Amazon](https://a.co/d/4PBEBoA) | [Ebay](https://www.ebay.com/sch/i.html?_from=R40&_trksid=p4432023.m570.l1313&_nkw=QCCAN+Interface+I2S+PCM5102A+DAC+Decoder+GY-PCM5102+I2S+Player+Module+pHAT+Format+Board+Digital+PCM5102+Audio+Board+for+Raspberry+Pi&_sacat=0)
- MicroSD Card (8GB+) - [Amazon](https://a.co/d/fdQJwdG) | [Western Digital](www.westerndigital.com/products/memory-cards/sandisk-ultra-uhs-i-microsd)
- USB or SD card adapter for the MicroSD card if your computer does not have a MicroSD slot (your MicroSD card may come with one, read its description) - Easily found on Amazon
- 3D printed case (use the case and cover STL files in the repository [coming soon])

<div align="center"><h2>Tools</h2></div>

- 3D Printer
- Soldering Iron
- Solder (preferably rosin core, lead-free)
- Jumper wires (if your DAC decoder does not come with them, mine did)
- Third hand tool/PCB holder (optional but recommended)
- Hot glue & glue gun
- Metal file (or something else to shave down the HDMI port's overhang)

<div align="center"><h2>Instructions</h2></div>

<br/><br/>

<div align="center"><h3>- 1 -<br/>Gather the parts and tools specified</h3></div>

When 3D printing the case, I recommend a small layer height as opposed to a larger one for finer detail on port cutouts. I also recommend enabling supports.

<br/><br/>

<div align="center"><h3>- 2 -<br/>Prepare the Pi to fit into the 3D-printed case</h3></div>

Discard the black piece of plastic covering the flex cable connector on the short edge of the Pi.

File away the metal on the HDMI port that overhangs the edge of the PCB board (notice in the second image how the HDMI port cannot be seen from underneath).

<br/>

<div align="center">
  <img src="./images/IMG_8657.jpeg?raw=true" alt="Pi preparation1" width="47%" />
  <img src="./images/IMG_8658.jpeg?raw=true" alt="Pi preparation2" width="47%" />
</div><br/>

<br/><br/>

<div align="center"><h3>- 3 -<br/>Solder jumper wires to the DAC decoder</h3></div>

On the short side of the board, the pads `BCK`, `DIN`, `LCK/LRCK`, `GND`, and `VIN` need to have jumper wires soldered to them. Do NOT solder a wire to `SCK`.

Apply a small bead of solder to jump the two little pads on the front side of the board, between `SCK` and `BCK`. **NOT THE `SCK` AND `BCK` PADS THEMSELVES, but the tiny pads next to them.**

<br/>

<div align="center">
    <img src="./images/IMG_8656.jpeg?raw=true" alt="DAC connections" width="500" />
</div><br/>

<br/><br/>

<div align="center"><h3>- 4 -<br/>Solder the jumper wires from the DAC to the Pi</h3></div>

Make sure the wires go into the front component side of the Pi, otherwise the Pi will not fit into the case properly. Please use the following guide:

<table align="center">
    <head><tr>
        <th>Connection Guide</th>
        <th>GPIO Labels</th>
    </tr></thead>
    <tbody><tr>
        <td>
            <pre>DAC BOARD   &gt; Raspberry Pi Zero 2 W connector
-----------------------------------------------
SCK         &gt; Not wired (Internally generated)
BCK         &gt; PIN 12    (GPIO18)
DIN         &gt; PIN 40    (GPIO21)
LCK/LRCK    &gt; PIN 35    (GPIO19)
GND         &gt; PIN 6     (GND) Ground
VIN         &gt; PIN 2     (5V)</pre>
        </td>
        <td>
            <img src="./images/IMG_8659.jpeg?raw=true" alt="GPIO guide" width="400" />
        </td>
    </tr></tbody>
</table>

<div align="center"><h4>How mine looked after this step</h4></div>

<div align="center">
    <img src="./images/IMG_8662.jpeg?raw=true" alt="Pi connections" width="500" />
</div><br/>

<br/><br/>

<div align="center"><h3>- 5 -<br/>Flash the SD card</h3></div>

<div align="center"><i>Before permanently hot gluing the Pi and DAC into the 3D printed case, it is a good idea to make sure it all works, in case any fixes need to be made.</i></div><br/>

On your computer, plug the MicroSD card into your MicroSD reader slot or adapter.

Download the Raspberry Pi imager from the [Raspberry Pi software page](https://www.raspberrypi.com/software/).

Launch the application and grant the necessary permissions. You should see a screen for selecting a device, an operating system, and storage.

For the device, choose `RASPBERRY PI ZERO 2 W`. The operating system should be `RASPBERRY PI OS (LEGACY, 64-BIT) LITE` (this may be located in the `Raspberry Pi OS (other)` category).

Select your MicroSD card or slot name from the Storage options, mine was called "APPLE SDXC READER MEDIA".

<br/>

<div align="center">
    <img src="./images/IMG_8674.png?raw=true" alt="Raspberry Pi flash software" width="500" />
</div><br/>

Click `NEXT`. If it asks you if you would like to apply OS customization settings before it flashes the MicroSD card, I recommend setting a custom hostname, username, and password. You can do this by clicking `EDIT SETTINGS`.

<br/>

<div align="center">
    <img src="./images/IMG_8675.png?raw=true" alt="OSC edit settings" width="500" />
</div><br/>

Go to the `GENERAL` tab. The hostname is what will show up on your Apple device when connecting to the AirPlay server, I named mine "AirPlayRBP", but if you leave it as default, it will be called "raspberrypi".

<br/>

<div align="center">
    <img src="./images/IMG_8676.png?raw=true" alt="OS customization general" width="500" />
</div><br/>

Go to the `SERVICES` tab and tick the `Enable SSH` checkbox.

<br/>

<div align="center">
    <img src="./images/IMG_8677.png?raw=true" alt="OS customization services" width="500" />
</div><br/>

Click `SAVE`. Now when it asks if you would like to apply OS customization settings, click `YES`.

<br/>

<div align="center">
    <img src="./images/IMG_8678.png?raw=true" alt="OSC yes" width="500" />
</div><br/>

If it asks if you would like to prefill the WiFi password from the system keychain, click `No`. We will set this up manually.

<br/>

<div align="center">
    <img src="./images/IMG_8679.png?raw=true" alt="WiFi keychain" width="500" />
</div><br/>

When it asks if you still want to continue, click `YES`. It may take a few minutes to flash the MicroSD, and you may be prompted to authenticate your computer accessing the MicroSD card.

When the program is finished flashing the card, you can close it.

Unplug the MicroSD card from your computer and plug it back in.

<br/><br/>

<div align="center"><h3>- 6 -<br/>Configure the network settings</h3></div>

The SD card should be called `bootfs`, and navigating to it with a file explorer will reveal your Pi's boot directory.

Create a new file there called `wpa_supplicant.conf` using your preferred text editor. Open the file and paste in the template below:

<pre><code>ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=&lt;YOUR COUNTRY CODE&gt;

network={
    ssid="&lt;YOUR 1ST NETWORK NAME&gt;"
    psk="&lt;YOUR 1ST NETWORK PASSWORD&gt;"
    key_mgmt=WPA-PSK
    id_str="&lt;YOUR 1ST NETWORK ID&gt;"
    priority=&lt;YOUR 1ST NETWORK PRIORITY&gt;
}

network={
    ssid="&lt;YOUR 2ND NETWORK NAME&gt;"
    psk="&lt;YOUR 2ND NETWORK PASSWORD&gt;"
    key_mgmt=WPA-PSK
    id_str="&lt;YOUR 2ND NETWORK ID&gt;"
    priority=&lt;YOUR 2ND NETWORK PRIORITY&gt;
}</code></pre>

Fill in your personalized preferences for each WiFi network you wish to add (anything with angular brackets `<...>`).

- `country`: the two-letter country code (i.e. `country=US`, `country=AU`, etc.)
- `ssid`: the case-sensitive name of the WiFi network (surrounded by double quotes)
- `psk`: the password (surrounded by double quotes
- `id_str`: a unique label to distinguish from other networks, could be anything (surrounded by double quotes)
- `priority`: a unique number to determine what order WiFi networks should connect in (integer, no quotes)

This configuration is for two WiFi networks, but a different number of networks can be configured by adding or removing `network={...}` listings.

Save and close `wpa_supplicant.conf`.

<br/>

<div align="center"><h4>Useful Network Settings Info</h4></div>

Once the Pi boots for the first time, the `wpa_supplicant.conf` file will be digested by the operating system, and it will be deleted

Don't worry! These network settings are not permanent! Here are a few ways to change them later down the line...

- **Running the `wpa_cli` command with root privileges in the Pi's terminal** (interactive way to adjust existing settings, i.e. add a new network, or change a password). Here are a few useful commands to run after opening interactive mode with `sudo wpa_cli`:
    - `list_networks` gives you networks with their ID numbers (useful in other commands)
    - `get_network <network_number> <variable_name>` gets the value of a variable from a specific network (i.e. `get_network 0 ssid`)
    - `set_network <network_id> <variable_name> <value>` sets the value of a variable of a specific network (i.e. `set_network 0 psk "S1lly_p455c0d3"`)
- **Creating a new `wpa_supplicant.conf` file in the boot directory** (overwrites the existing network settings). This is useful if you cannot SSH into the Pi for various reasons.

<br/><br/>

<div align="center"><h3>- 7 -<br/>Boot up the Pi</h3></div>

Plug the MicroUSB power cable into the outermost MicroUSB port. Two lights should turn on, a green LED on the Pi, and a red LED on the DAC. It will take several minutes for the Pi to configure itself on the first boot, so be patient.

<div align="center"><i>A good way to tell if the Pi is finished booting is if the green Pi LED is steady</i></div><br/>

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

<div align="center"><h2>Resources</h2></div>
- Code to run AirPlay server: [Shairport Sync](https://github.com/mikebrady/shairport-sync)
- DAC jumper wire connections: [Kamran Sethi](https://raspberrypi.stackexchange.com/a/76264)
