# Using Android as an External GPS

Kismet supports logging GPS information while it is monitoring wireless traffic which can be useful for mapping out AP locations and signal heat maps. To do this, you can use a dedicated GPS device, or leverage the GPS built into most Android devices. The following process describes how to set up a Kali environment so an Android smartphone can provide GPS information over USB to a Kali machine.

## Setting Up

1. Install the latest version of JDK
   1. [Download the JDK ](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
   2. Move the JDK to the desired installation location \(eg. /etc\)
   3. Extract the tarball
      1. `tar zxvf jdk-version-linux-x64.tar.gz`
   4. Delete the tarball to save disk space
      1. `rm jdk-version-linux-x64.tar.gz`
2. Install Android SDK
   1. [Download the Android SDK command line tools](https://developer.android.com/studio/index.html#downloads)
   2. Create a directory in the desired installation location \(again, /etc\)
      1. `mkdir /etc/android-tools`
   3. Move the download zip to the new folder
      1. `mv tools_version-linux.zip /etc/android-tools`
   4. Extract the tools from the .zip
      1. `unzip /etc/android-tools/tools_version-linux.zip`
   5. Create the ANDROID\_HOME environment variable
      1. `export ANDROID_HOME=/etc/android-tools`
   6. Make the environment variable permanent
      1. insert the above export line into ~/.bashrc using your favourite text editor
   7. Run the Android SDK Manager
      1. `/etc/android-tools/tools/android`
   8. Install all tools packages
3. Allow USB debugging on the Android device
   1. Go to _Settings&gt;Development Options_
   2. Enable USB debugging
4. Set up ADB
   1. Connect the Android device
      1. Select PTP connection \(for image transfer\)
      2. Allow USB debugging or the debugging device's fingerprint
   2. Get vendor/device info
      1. `lsusb`
      2. Bus 001 Device 019: ID **04e8:6866** Samsung Electronics Co., Ltd GT-I9300 Phone \[Galaxy S III\] \(debugging mode\)
   3. Create Android rules file and add the following
      1. `nano /etc/udev/rules.d/512-android.rules`
      2. `#Samsung`
         `SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", ATTR{idProduct}=="6866", MODE="0666", GROUP="plugdev"`
      3. Where \_\#Samsung \_is the vendor's name, and the idVendor and idProduct fields are taken from the emboldened part of the lsusb output.
   4. Add the vendor to adb\_usb.ini
      1. `nano ~/.android/adb_usb.ini`
      2. Under the commented section, add:
         1. 0x04e8 \(see idVendor above\)
   5. Update ADB
      1. `/etc/android-tools/tools/android update adb`
   6. Resart adb server
      1. `/etc/android-tools/platform-tools/adb kill-server`
      2. `/etc/android-tools/platform-tools/adb start-server`
   7. Check to see it all worked
      1. `etc/android-tools/platform-tools/adb devices`
      2. `List of devices attached`
         `1234567890ABCDEF    device`
5. Create udev rules  
   1. Identify the phone's identity  
      1. Unplug the phone, then run the following monitoring command:  
         1. `sudo udevadm ==property | grep ID_MODEL=`  
      2. Plug the phone back in and capture the output, eg:

   1. `ID_MODEL=SAMSUNG_Android`  
      2. Create the 65-adb\_gps\_\_\_usb.rules

      1. `nano /etc/udev/rules.d/65-adb_gps_usb.rules`

      2. Enter the following:

   2. `# udev rules for start shared android gps device`

      `LABEL="adb_gps_rules"`

      `ACTION=="add", ENV{ID_MODEL}=="SAMSUNG_Android", ENV{ADBGPS}="true", RUN+="/usr/local/bin/adb_gps.helper start"`

      `ACTION=="remove", ENV{ADBGPS}=="true", RUN+="/usr/local/bin/adb_gps.helper stop"`

      `LABEL="adb_gps_rules_end"`

      1. Refresh udev configuration

   3. `udevadm trigger`

      1. Put the following scripts into /usr/local/bin

   4. [adb\_gps.helper](http://www.jillybunch.com/sharegps/files/adb_gps.helper)

   5. [adb\_gps\_\_\_usb.sh](http://www.jillybunch.com/sharegps/files/adb_gps_usb.sh)

      1. Change ownership and permissions of these files

   6. `chown root /usr/local/bin/adb_gps*`

   7. `chmod 755 /usr/local/bin/adb_gps*`

6. Run ShareGPS on the Android device

   1. Open ShareGPS

   2. Go to the connections tab and press add

   3. Set the data type to "Standard NMEA Format"

   4. Set the connection method to USB

   5. Name the connection

## Get Tracking

Now both the Kali box and Android device set up, get both sides running and talking to each other.

1. Tap on the connection to start the listener on the Android device

2. Run GPSD to listen to the NMEA connection

   1. Forward the port from the USB port to another port

      1. `/etc/android-tools/platform-tools/adb forward tcp:20175 tcp: 50000`

   2. Start GPSD listening to the USB connection

      1. With extensive logging

         1. `gpsd -N -n -D5 tcp://localhost:20175`

      2. Or, in the background

         1. `gpsd tcp://localhost:20175`

      3. After a moment, ShareGPS should show the connection status as connected.

3. Start Kismet

   1. `kismet`

   2. If you gave a GPS position, it will be visible in the interface

   3. If not, test that the GPS is being read

      1. In Kismet, go to Windows &gt; GPS Details

      2. After a moment, a list of connected satellites and signal strengths should appear.

## Mapping GPS Data

### Mapping with giskismet and Google Earth

1. Make sure you have giskismet installed:
   1. `apt-get update`
   2. `apt-get install giskismet`
2. Convert the Kismet output to KML
   1. `giskismet -x Kismet-DATE.netxml -q "select * from wireless" -o output.kml`
3. Open the kml file in Google Earth
   1. File \| Open \| output.kml

### Mapping with NetXML-to-CSV and Google Maps

1. Download NetXML-to-CSV
   1. `git clone https://github.com/MichaelCaraccio/NetXML-to-CSV.git`
2. Convert the Kismet output
   1. python3 main.py Kismet-DATE.netxml result output.csv
3. Open the csv in Google Maps
   1. Go to [https://www.google.com/maps/d/](https://www.google.com/maps/d/)
   2. Create a new map
   3. Import the csv file

### Resources

[http://www.jillybunch.com/sharegps/user\_linux.html](http://www.jillybunch.com/sharegps/user_linux.html#USBTETHERING)

[https://androidonlinux.wordpress.com/2013/05/12/setting-up-adb-on-linux/](https://androidonlinux.wordpress.com/2013/05/12/setting-up-adb-on-linux/)

