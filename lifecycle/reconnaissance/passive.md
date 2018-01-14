# Passive Information Gathering

## OSINT - Open Source Intelligence

Before you even start poking around at servers, there's plenty of information available online about your target thay will likely help you in later stages of testing. This information is collectively known as Open Source Intelligence, or OSINT, and it is your friend.

## WhoIs Enumeration

When a domain name get's registered, the details of the registration are recorded and are often publicly searchable. Some details are accessible from the command line, and others can only be viewed from a WhoIs webpage.

```
whos example.com
```

## DNS Enumeration

### Forward DNS Lookup

DNS Servers contain records which map domain names to their respective IP address(es). A forward DNS lookup is a query to a DNS containing a domain name, and the DNS returns the IP address of the server hosting that domain.

```
nslookup example.com
```

### Reverse DNS Lookup

DNS servers can also contain PTR records, which map allow for lookups on IP addresses in order to retrieve a domain name. Reverse DNS lookups can be useful over an IP range to find which domains are hosted within that range.

```
dnsrecon -r 192.168.0.0/24
```

### Zone Transfer

The internet relies heavily on a network of DNS servers to maintain a consistent manifest of DNS records. A zone transfer is a mechanism to copy the records from a master DNS to a secondary DNS. If this is allowed for non-trusted secondary DNSs, zone transfers can be used to dump the contents of an enterprises internal DNS records, revealing sensitive information about their internal network.

```
dnsrecon -d example.com
```

### Other DNS Tools

```
sublist3r -v -d example.com         - Search for subdomains under a target domain.
dnsrecon -asgwz -d example.com      - Perform a number of DNS scans agains the domain.
```

## Search Engines

### The Harvester

The harvester is a fantastic tool that automates searching different search engines for email addresses, subdomains and other information about your target domain.

```
theharvester -d example.com -b all -l 200
```

### Google Hacking
 * **site:"domain.com"** - Search pages in a certain domain
 * **-site:"domain.com"** - Search for pages not in the domain
 * **intitle:"string"** - Search for sites with the string in the title
 * **inurl:"string"** - Search for sites with the string in the URL

## Useful Links

[AusRegistry WhoIs](https://ausregistry.com.au/whois) - A web-based WhoIs query tool.

[DNS Dumpster](https://dnsdumpster.com/) - Simple lookup for DNS records.

[Google Hacking Database](https://www.exploit-db.com/google-hacking-database/) - A database of Google queries whihc help you search for information about or within sites.

[Netcraft](https://www.netcraft.com/) - Information about websites including DNS records and web application servers and frameworks being used.

[OSINT Framework](http://osintframework.com/) - Looking for a particular type of information? This site will help you find a tool or service which will help you get it.

[Shodan](https://www.shodan.io) - Get information about ports / services running on IP addressed, subdomains and domains. The browser extension is pretty handy too.