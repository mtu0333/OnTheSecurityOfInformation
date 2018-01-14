# Active Information Gathering 

## Port Scanning

### TCP Scanning

TCP scanning aims to identify which TCP ports on a machine are open and respond to network traffic. There are two common types of scan which acheive this:
  * TCP Connect Scan: Attempt to create a connection to a given port on the target machine. This includes completing the 3-way handshake required to form a TCP connection.
  * TCP SYN Scan: Initiate a TCP connection handshake with the target, but do not send the final ACK packet. It is not required to identify that the port is open.

```
nmap example.com              - Basic scan
nmap -p- example.com          - All ports
nmap -A example.com           - Automatically enables a set of other options which do extended scanning.
```

### UDP Scanning

UDP is a connectionless protocol, therefore requires a different method for port scanning to TCP. When a UDP packet is sent to a closed port, the target host will usually reply with an _ICMP Unreachable_ packet to indicate that there is no service listening on that port. If there is a service, no packet is returned. A UDP port scan will note which ports return an ICMP Unreachable packet and which do not, which may indicate which ports are open. However, a many firewalls will drop ICMP packets to prevent UDP scans, making UDP scans unreliable as a means for identifying open UDP ports.

```
nmap -sU example.com          - Scan most common UDP ports
nmap -sU -p-                  - Scan all UDP ports
```

## SSL / TLS configuration testing

Where services provide encryption over SSL / TLS, such as HTTPS, it is useful to scan the host to test for any configuration issues that may weaken the security of these services. Some useful tools to test SSL / TLS are:

```
sslscan <IP/Hostname[:port]>
sslyze --regular <IP/Hostname[:port]>
nmap -p<port> --script=ssl-cert,ssl-enum-ciphers <IP/Hostname>
```

## Useful Sites
