---
title: "DNS"
description: ""
summary: "DNS translates human readable domain names into machine readable addresses"
date: 2023-02-13T20:15:31-08:00
lastmod: 2023-02-13T20:15:31-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "dns-810a65fbfe2eb2a38e36fb21b7be230b"
weight: 999
toc: true
---

Domain Name System (DNS) is the phone book of the internet. It translates human friendly domain names, e.g., `www.exmaple.com`, to computer friendly addresses like `192.168.1.1`.

## Basic Overview

Domain names (like `github.com`) are made for humans. They're nice and easy for us to use and remember. Websites, however, live at IP addresses (like `140.82.114.4`), which are not easy to remember. DNS is the system that tells the browser to fetch `140.82.114.4` when the user requests `github.com`.

- DNS is distributed
- DNS is hierarchical

## DNS Record Types

DNS record types provide information about the domain.

| Type  | Name               | Description                                               |
| ----- | ------------------ | --------------------------------------------------------- |
| A     | A Record           | Points domain name to IP address                          |
| AAAA  | AAAA Record        | Points domain name to IPv6 address                        |
| CNAME | Canonical name     | Points domain name to another domain name                 |
| NS    | Nameserver         | Specifies the authoritative DNS server for a given domain |
| MX    | mail exchange      | Specifies where emails for a domain should be routed to   |
| SOA   | Start of Authority | Stores admin information about a domain                   |

## Basic overview of DNS for website resolution

<!-- ![DNS routing](./assets/dns-Page-2.png) -->

1. Browser requests DNS record from **DNS resolver**. DNS Resolver is typically your ISP.
1. If the DNS Resolver comes up dry (does not have it in cache) it will reach out to the **root server**. Root server is the top level of the DNS heirarchy. 13 servers are strategically placed around the world.
   - If the Resolver has the record cached, the process will skip to step 8
1. Root server does not contain the IP address, but instead directs the Resolver to the TLD server
1. The resolver asks the TLD server fot the IP address. The TLD server stores the IP addresses for top level domains (like `.com`).
1. The TLD server does not know the IP address, but will direct the Resolver to the **Authoratative Name Server**.
1. The Resolver will ask the name server for the IP address. The name server should contain everything about the domain, including its IP address.
1. The Name Server return the IP address for github.com to the Resolver, which caches it in memory.
1. The resolver returns the IP address to the client. The browser can now make the request to the web page.
1. The browser requests the webpage from github.com's web server
1. The web server returns the age to the client

### TTL (Time to Live)

TTL is a parameter on a DNS record that tells a resolver how long to cache the record before requesting a new one.

Caching a record is a double edged sword. When a DNS record is cached in a resolver, it does not need to go looking for the DNS record, increasing the speed for the end user. However, if the DNS record changes, the resolver may continue to send traffic to the old, cached address.

DNS propogration is waiting for the cached DNS records to expire (where the resolver will request new DNS records with the updated answer).

## Split Horizon DNS

Information to follow.
