---
tags:
  - system_design
  - "#component"
---
# Domain Name System

The Domain Name System (DNS) is the the Internet's naming service that maps domain names to IP addresses that computers can understand.

When we access a domain name, we make a request of that domain name to the DNS server that then returns associated IP addresses with that domain name. 

### Name Server
A DNS server that responds to user queries is called a **name server**

### Resource Records
Resource records are what's stored in DNS database stores, they are the smallest "unit" of DNS information that is requestable from a name server

## Resource Record Types
| **Type** | **Description** | **Name** | **Value** | **Example (Type, Name, Value)** |
| ---- | ---- | ---- | ---- | ---- |
| A | Provides the hostname to IP address mapping | Hostname | IP address | (A, relay1.main.educative.io,104.18.2.119) |
| NS | Provides the hostname that is the authoritative DNS for a domain name | Domain name | Hostname | (NS, educative.io, dns.educative.io) |
| CNAME | Provides the mapping from alias to canonical hostname | Hostname | Canonical name | (CNAME, educative.io, server1.primary.educative.io) |
| MX | Provides the mapping of mail server from alias to canonical hostname | Hostname | Canonical name | (MX, mail.educative.io, mailserver1.backup.educative.io) |

## DNS Hierarchy 
DNS name servers come in four types that make up the DNS hierarchy, and they all help route a request to the right place.
1. **DNS Resolver**
	Starts the querying process and forwards requests to other DNS name servers
2. **Root-level Name Server**
	Receive requests from local servers, and handle requests based on the top level domains, e.g. `.edu`, `.com`, `.io`. 
3. **Top-level Domain (TLD) Name Server**
	 Gets the IP addresses of the authoritative name servers in that given domain
4. **Authoritative Name Server**
	These hold the given organization's DNS name servers that provide the actual IP addresses

## Caching 
Caching in the context of DNS refers to the act of saving commonly accessed resource records for future reference. Caching can be done at different hierarchy levels to increase efficiency of DNS lookups.