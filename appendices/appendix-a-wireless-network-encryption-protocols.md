# Appendix A - Wireless Network Encryption Protocols

Due to the nature of wireless networks, they are natively less secure than their wired counterparts, so additional efforts must be made to ensure sensitive communications are kept private. An analogy about the difference between wired networks \(such as copper or fibre\) and wireless networks \(such as Wi-Fi or cellular\) is as follows: 

Two friends are sitting at opposite ends of a crowded cafe, and they wish to share a secret. The "wired" method would be to write a note and put it in an envelope, pass it to the waiter/waitress and ask them to hand it to the other friend. Somebody could steal the letter and open it, but most of the people in the cafe wouldn't know it even happened. The "wireless" method would be to shout the secret to the friend and hope nobody in the cafe was listening or cared. Maybe the folks down the streed didn't hear the secred, but everybody in the cafe couldn;t help but eavesdrop. To keep the secret safe, the friends had better speak in code. Enter encryption protocols. 

## Wired Equivalent Privacy \(WEP\)

The purpose of WEP was, is its name suggests, to acheive the privacy of a wired connection over a wireless network. However, due to flaws in the protocol and the state of US policies on encryption at the time of the protocol's creation, WEP is susceptible to a number of attacks and is widely considered vulnerable and deprecated today.

The security issues with WEP stem from US export restrictions on cryptographic technologies in the '90s, which dictated that encryption keys could be no longer than 64-bits. Of this 64-bits the original WEP standard uses 24 bits for the Initialisation Vector \(IV\) leaving 40-bits for the WEP key itself. Once the restrictions were lifted, the total key length was lengthened to 128-bits, but the protocol still used a 24-bit IV.

Two types of authentication can be used with WEP:

1. Open system authentication: no credentials are required to join, so no actual authentication is achieved and any client can associate with the network. The client simpy recieves an authentication token to authenticate for the duration of the session. Packets are then encrypted using a WEP key \(either a hexadecimal key or in some cases a passphrase\) and if the WEP key does not match the WEP key on the router, the packet is simply dropped. TL;DR, anybody can associate, only clients with the actual WEP key can communicate.
   1. The client requests authentication to the AP.
   2. The AP generates an authentication code and sends it to the client.
   3. The client accepts the authentication code and uses the code to authenticate for the remainder of the session. The client encrypts with the WEP key.
2. Shared Key Authentication: Shared key authentication uses a challenge response mechanism to authenticate associate a client with the network.
   1. The client sends an authentication request to the AP
   2. The AP sends back a clear text message
   3. The client encrypts the message with their WEP key and sends the encrypted message to the AP
   4. The AP attempts to decrypt the message. If the WEP key is correct, the message will decrypt and match the original, and the client is authenticated. If the WEP key is wrong, the message wil not correctly decrypt and the client is denied access.

While Open System Authentication seems less secure, as no authentication is achieved and any client can associate, it is actually more secure than Shared Key Authentication, as any client without the correck WEP key cannot communicate. Because both the unencrypted and encrypted messages can be captured in the SKA handshake, an offline attack can be performed to crack the WEP key.



