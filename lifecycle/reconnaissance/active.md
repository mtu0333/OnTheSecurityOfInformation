# Active Information Gathering

## WHOIS


## DNS Enumeration

**Forward DNS Lookup**

DNS Servers contain records which map domain names to their respective IP address(es). A forward DNS lookup is a query to a DNS containing a domain name, and the DNS returns the IP address of the server hosting that domain.

**Reverse DNS Lookup**

DNS servers can also contain PTR records, which map allow for lookups on IP addresses in order to retrieve a domain name. Reverse DNS lookups can be useful over an IP range to find which domains are hosted within that range.

**Zone Transfer**

The internet relies heavily on a network of DNS servers to maintain a consistent manifest of DNS records. A zone transfer is a mechanism to copy the records from a master DNS to a secondary DNS. If this is allowed for non-trusted secondary DNSs, zone transfers can be used to dump the contents of an enterprises internal DNS records, revealing sensitive information about their internal network.


## Port Scanning

**TCP Scanning**

TCP scanning aims to identify which TCP ports on a machine are open and respond to network traffic. There are two common types of scan which acheive this:
  * TCP Connect Scan: Attempt to create a connection to a given port on the target machine. This includes completing the 3-way handshake required to form a TCP connection.
  * TCP SYN Scan: Initiate a TCP connection handshake with the target, but do not send the final ACK packet. It is not required to identify that the port is open.

**UDP Scanning**

UDP is a connectionless protocol, therefore requires a different method for port scanning to TCP. When a UDP packet is sent to a closed port, the target host will usually reply with an _ICMP Unreachable_ packet to indicate that there is no service listening on that port. If there is a service, no packet is returned. A UDP port scan will note which ports return an ICMP Unreachable packet and which do not, which may indicate which ports are open. However, a many firewalls will drop ICMP packets to prevent UDP scans, making UDP scans unreliable as a means for identifying open UDP ports.