# Information Gathering

## Check Out the Application

First things first, if you are going to be testing the security of a web application, a good place to start is to navigate to any URLs provided to you by whoever requested the pentest. This will give you a feel for what you're attempting to break into.

## Search Engine Discovery

Following that, search for the site in a search engine. Try using advanced searches such as _site:www.example.com \_which narrows results to those from that domain, and \_cache:www.example.com_ to examine the current cached version of the site. Look out for any interesting information, such as email address conventions, network diagrams, error messages, and anything that looks like an artefact that has been made available online at some point which may help you further in the pentest.

## Web Server Fingerprinting

In addition to the web application itself, the web server hosting the application is within the scope of a web application pentest as a compromise of the web server can lead to a compromise of the application, such as a denial of service. There are a number of different manual approaches and automated tools which can help fingerprint a web server.

* HTTP response server header: If the web server is not securely configured, the web server will advertise itself in HTTP response headers. The following header shows the server is Apache 1.3.23.

![](/assets/server-header.png)

* HTTP response header order: Different web servers and version will order HTTP headers differently in their responses. This information can be used to identify the server and version it is running.

* HTTP bad request response: Likewise, different web servers will respond to bad requests differently. Try sending a bad request and see what the response tells you.

Of course, there are tools which are pre-loaded with many known HTTP responses and will automatically send HTTP requests and analyse responses and attempt to identify the web server software version.

* **HTTPrint** - A command line tool which will probe a web server and compare responses to a file full of signatures. There is also a Win32 GUI version of the same tool.

  ```
      httprint -h <ip-address> -s /usr/share/httprint/signatures.txt
  ```

* **Netcraft **- An online tool which retrieves information about a website and its web server, such as domain information, scripting frameworks, and PaaS/IaaS usage.

## Web Application Enumeration

When doing a web application penetration test, clients will typically provide a list of IP addresses or domain names as a part of the scope. Due to virtual hosts, there isn't necessarily a 1:1 ratio between IP addresses and web applications. Depending on the arrangement, a penetration tester may be asked to target every web application hosts on one of these IPs. It's then the job of the pentester to enumerate the web servers on the servers. There are three ways multiple applications can be hosted on a web  server:

* **Different base URL** - One website could be located at _www.example.com/site1_ and another located at _www.example.com/site2_.
  * If the web server allows directory browsing, it may be possible to spot these applications.
  * The applications may have been spidered by a search engine or using a tool like BurpSuite.
  * Brute force common URLs.
* **Non-standard ports** - Web servers typically serve HTTP on port 80 and HTTPS on port 443, however they can serve on other ports. Ports 8000 and 8080 are two other common HTTP ports, however any other port could be used.
  * Scan open ports using a tool like NMAP to see if there are any other web services running.
* **Virtual Hosts** - A single IP address can be associated with multiple DNS names, such as _example.com_ and _helpdesk.example.com_. When making requests to a web server, the Host header dictates which virtual host the client is referring to.
  * A domain transfer may disclose a number of DNS entries for the domain. `Fierce`is a a good tool which automates this and also brute forces common DNS names.
    * `fierce -dns <domain name>`
  * Try a DNS inverse query


## Useful Links

[**How to control web page caching across all browsers**](https://stackoverflow.com/questions/49547/how-to-control-web-page-caching-across-all-browsers) - A useful description of cache-control headers and how to set them in code.


