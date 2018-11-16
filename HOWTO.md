#How to set up a Raspberry Pi Wixel receiver#

>Based on using the Raspbian distribution.

First login to your Pi and set it up with a .local name, see: [assigning a .local address](http://www.howtogeek.com/167190/how-and-why-to-assign-the-.local-domain-to-your-raspberry-pi/)

Make sure packages are installed:

`sudo apt-get update && sudo apt-get -y install git screen python python-pycurl python-serial wget sdcc`

Download the usb wixel python code:

`git clone https://github.com/Mendanbar/python-usb-wixel-xdrip.git`

`cd python-usb-wixel-xdrip`

Run the auto-installer:

`sh forusb.sh`

This will compile the wixel firmware and install the python script

If all is good you should see "Waiting for connections" on the screen 
and a flashing yellow light on the Wixel until it gets the first dex signal.

xDrip+ [versions after 20th Feb 2017](https://github.com/NightscoutFoundation/xDrip/releases) allow for easy setup by accessing the `System Status` -> `IP collector` page and your raspberry pi should be automatically detected. Tapping on its address on this page allows you to automatically configure the settings if you have followed the instructions above for [assigning a .local address](http://www.howtogeek.com/167190/how-and-why-to-assign-the-.local-domain-to-your-raspberry-pi/)

Alternatively, within [xDrip+](https://jamorham.github.io/#xdrip-plus) select `WiFi Wixel` in `Data source` settings section and add `raspberry.local:50005` in to `List of Receivers` or whatever your Raspberry Pi hostname is.

Using `raspberry.local:50005` may not work on all android devices, if it doesn't try instead using the IP address, eg `192.168.5.123:50005` in the `List of Receivers` replacing with the real ip address.

After reboot you can check the script is running with:
`sudo screen -r wixel`

#How to change the transmitter#

Make note of the new transmitter ID on the back of the box/device

Open the wixel-xDrip github project

on line 139 of apps/dexdrip/dexdrip.c, change it to:
static CODE const char transmitter_id[] = "XXXXX";
where XXXXX is the new id you noted above

log in to each of the RpiWixels [currently fullpageos and zeroWixel]

'ssh [rpi_name] -l [login]'

clear the wixel-xDrip dir and rebuild

cd python-usb-wixel-xdrip
rm -rf wixel-xDrip
sh forusb.sh

If prompted, enter the new transmitter ID again

SUCCESS, we have a working wixel!  Now to swap out the transmitter.

Stop the sensor on the classic dexcom receiver

scroll down to settings> transmitter id and input the new one

replace the old transmitter with the new

Finally, change the source in the xDrip android app ( in 'List of Receivers') to:
http://wixel-receiver.appspot.com/XXXXX/12345/json.get

where XXXXX is the new transmitter id

Now you should start getting readings in xDrip 

