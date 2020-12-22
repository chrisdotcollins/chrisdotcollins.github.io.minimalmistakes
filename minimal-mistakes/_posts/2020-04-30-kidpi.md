---
title: Raspberry Pi for Kids
subtitle: Safer internet sessions for kids
date: 2020-04-30
categories: ["pi", "kids"]
---

## Introduction
The following are some ways of 'securing' a Pi for use by young kids - it's not about trust but more about protecting from any accidental clicks!

## Using OpenDNS Family Shield
[OpenDNS Family Shield](https://www.opendns.com/setupguide/#familyshield) limits access to 'unsavoury' sites by blocking access to blacklisted IP addressess by intercepting DNS. Depending on the version of Linux used, you need to set DNS servers specifically to OpenDNS Family Shield IP addresses. In Raspbian this is most likely via NetworkManager:
* ```208.67.222.123```
* ```208.67.220.123```

## Forcing the use of Google Safesearch

[Google Safesearch](https://support.google.com/websearch/answer/186669) forces all Google requests to be run with safesearch enabled.

* Enter the command ping forcesafesearch.google.com to identify the safe search IP addres (it will be something like 216.239.38.120).
* Add an entry in /etc/hosts file with the IP address with this IP address for Google, for example: ```216.239.38.120 www.google.com #forcesafesearch```
* Also add this line for any other Google country domain, e.g. www.google.co.uk.
* To confirm SafeSearch is on, go to google.com and check that SafeSearch is on by default and can't be turned off.



