# Wireless Penetration Testing - Introduction

## Wireless Interface Tools

There are a set of tools commonly used for managing network interfaces, such as `ifconfig`,`ifdown/ifup` and `ifquery`. Similarly, there is a set of tools dedicated to the management of wireless interfaces which are used heavily in wireless penetration testing.

```
iwconfig                        - list all wireless interfaces and basic information
iwconfig <interface> <option>   - configure wireless interfaces by setting the ESSID, channel etc
iwlist <interface> <option>     - get details about a wireless interface (channel, mode etc)
```

There are also a number of tools dedicated to monitoring wireless traffic received by a wireless adaptor. In order to listen to this traffic, the adaptor has to be in monitoring modem a promiscuous-like mode which will listen to all traffic from all APs and clients. This can be achieved using the `iwconfig`command.

```
iwconfig <interface-name> mode monitor channel <channel-number>
```

Once an interface is set to monitoring mode, verify the mode and the name of the monitoring interface \(this may be different to the original wireless interface\) using `iwconfig`and capture packets using `tcpdump.`

```
tcpdump -i wlan0mon -s 65000 -p
```

## Airmon-ng

Whilst `iwconfig`can be used to configure wireless interfaces and set the mode of an interface, `airmon-ng` is a part of the `aircrack-ng` suite of wireless tools which makes the management of monitoring wireless interfaces simpler. The tool is primarily used to identify any running processes which may interfere with wireless monitoring, as well as manage the modes of wireless interfaces.

Usages:

```
airmon-ng <check> [kill]        - check for and optionally kill interfering wireless processes
airmon-ng <start|stop> <interface> [channel] - Start or stop monitoring a wireless interface on a specified channel
```

Examples:

```
airmon-ng check                 - check for any services that may interfere with monitoring tools
airmon-ng check kill            - kill any processes found by check
airmon-ng start wlan0 2         - set the wlan0 interface to monitor mode on Wi-Fi channel 2
airmon-ng stop wlan0mon         - stop monitoring on the wlan0mon interface
```

## Airodump-ng

The `aircrack-ng` suite also includes `airodump-ng`, a tool used to capture wireless packets from a wireless interface in monitoring mode. Primarily, the tool captures packets as specified by the parameters passed to it and writes these packets to a file for inspection by another tool such as `aircrack-ng.`Additionally, `airodump-ng` captures information about all of the wireless access points \(APs\) and clients that it sees and writes with information out to a file.

Usages:

```
airodump-ng <options> <interface-name>
Options:    --write <prefix>    - select an output filename
            --ivs               - only capture initialisation vectors, handy for cracking WEP
            --gpsd              - use gpsd to track location information
            --output-format     - specify the output filetype - pcap, ivs, csv, fps, kismet, netxml
            ...                 - (See man pages for more options)
```

Examples:

```
airmon-ng start wlan0 2         - remember to set interface to monitoring mode
airodump-ng -c 2 -bssid DE:AD:BE:EF:CO:FE -w capfile1 --ivs wlan0mon
```

The example listens on Wi-Fi channel 2 on the wlan0mon interface for traffic to and from the AP with BSSID`DE:AD:BE:EF:CO:FE` only capturing the initialisation vectors and writes the findings to the file capfile1.

## Aireplay-ng

In many cases, in addition to sniffing wireless traffic for interesting packets, a wireless attack will require specially crafted packets to be injected into the network. These packets can serve a variety of purposes, such as deauthenticate clients from APs, execute a fake authentication attack, replay ARP requests as well as execute a variety of other attacks. Aireplay-ng currently supports the following attacks:

* Attack 0 - De-authentication
* Attack 1 - Fake aithentication
* Attack 2 - Interactive packet relay
* Attack 3 - ARP request replay attack
* Attack 4 - KoreKchopchop attack
* Attack 5 - Fragmentation attack
* Attack 6 - Cafe-latte attack
* Attack 7 - Client-oriented fragmentation attack
* Attack 8 - WPA migration mode
* Attack 9 - Injection test

Usages:

```
aireplay-ng <options> <replay-interface>
Options:    -<attack-number>    - select the type of attack
            -b <bssid>          - the BSSID of the AP
            -d <mac>            - MAC address of the destination
            -s <mac>            - MAC address of the source
            ...                 - (See man pages for more options)
```

Examples:

```
airmon-ng start wlan0 2         - remember to set interface to monitoring mode
aireplay-ng -0 1 -a DE:AD:BE:EF:CO:FE -c CO:FE:DE:AD:BE:EF wlan0mon                          - de-authnetication
aireplay-ng -1 6000 -q 10 -e "My Wi-Fi" -a DE:AD:BE:EF:CO:FE -h CO:FE:DE:AD:BE:EF wlan0mon   - fake authentication
aireplay-ng -3 -b DE:AD:BE:EF:CO:FE -h CO:FE:DE:AD:BE:EF wlan0mon                            - arp replay
aireplay-ng -4 -b DE:AD:BE:EF:CO:FE -h CO:FE:DE:AD:BE:EF wlan0mon                            - korekchopchop attack
aireplay-ng -5 -b DE:AD:BE:EF:CO:FE -h CO:FE:DE:AD:BE:EF wlan0mon                            - fragmentation attack
aireplay-ng -9 -e "My Wi-Fi" -a DE:AD:BE:EF:CO:FE wlan0mon                                   - injection test
```

## Aircrack-ng

The main purpose of wireless penetration testing is to test whether a wireless network is vulnerable to attacks which could grant aunauthorised access to an attacker. Once an attacker has access to the network, they could carry out further attacks on the clinets and services offered on that network. An important component of gaining access is retrieving the keys for the encryption protocol used by the AP.

Aircrack-ng is designed to serve that purpose, taking a captured WEP or WPA/WPA2-PSK key and attempts to crack it. Since WEP and WPA are fundamentally different protocols, the requirements for cracking these keys is different. Aircrack-ng offers two different modes: mode 1 for WEP cracking and mode 2 for WPA cracking.

Usage:

```
aircrack-ng [options] <capture-file>        - capture file can be in .cap or .ivs formats
```

Example:

```
aircrack-ng capfile
```

# Airgraph-ng

Airgraph-ng is a handy little utility that isn't included in the Aircrack-ng suite anymore but still appears to work on newer versions of Kali and is still floating around the interwebs. Since it's no longer included as standard, it has to be installed as follows:

1. Navigate to /opt
   1. `cd /opt`
2. Download airgraph-ng from subversion
   1. `svn co http://svn.aircrack-ng.org/trunk/scripts/airgraph-ng`
3. Change the permissions so the python script is executable
   1. `chmod +x /opt/airgraph-ng/airgraph-ng`
4. Create a link so the script can be run without the path
   1. `ln -s /opt/airgraph-ng/airgraph-ng /usr/bin/airgraph-ng`

## To Add:

* Wash
* Pixie dust
* Pixie dust long
* Bully
* Reaver



