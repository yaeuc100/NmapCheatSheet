# NmapCheatSheet
this repo is solely for the purpose of education and should not be used for private reasons


# Target Selection
Scan a single IP:

nmap 192.168.0.1


# Scan a host:

nmap www.testhostname.com

# Scan a range of IPs:

nmap 192.168.0.1-20
# Scan a subnet:

nmap 192.168.0.0/24
# Scan targets from a text file:

nmap -iL list-of-ips.txt


# Port Selection
# Scan a single port:

nmap -p 22 192.168.0.1
Scan a range of ports:

nmap -p 1-100 192.168.0.1
# Scan 100 common ports:

nmap -F 192.168.0.1
# Scan all ports(65535):

nmap -p- 192.168.0.1
# Specify UDP or TCP scan:

nmap -p U:137,T:139 192.168.0.1
# Scan types
# Scan using TCP connect

nmap -sT 192.168.0.1
# Scan using TCP SYN scan

nmap -sS 192.168.0.1
# Privileged access is required to perform the default SYN scans

# Scan UDP ports

nmap -sU -p 123,161,162 192.168.0.1
# Scan selected ports (ignore discovery):

nmap -Pn -F 192.168.0.1
# Ignoring discovery is often required as many firewalls or hosts will not respond to PING, so could be missed unless you select the -Pn parameter.

## Service and OS Detection
# Detect OS and Services:

nmap -A 192.168.0.1

# Standard service detection:

nmap -sV 192.168.0.1
# Aggressive Service Detection:

nmap -sV --version-intensity 5 192.168.0.1
# The aggressive service detection is helpful when there are services running on unusual ports.

# Light banner detection:

nmap -sV --version-intensity 0 192.168.0.1
# Output Formats
# Save default output to file

nmap -oN outputfile.txt 192.168.0.1
# Save results as XML

nmap -oX outputfile.xml 192.168.0.1
# Save results in a format readable by  grep

nmap -oG outputfile.txt 192.168.0.1
# Save in all formats

nmap -oA outputfile 192.168.0.1
# Using the -oN option allows the results to be saved but also can be monitored in the terminal as the scan is under way.

# Scripting Engine


# The Nmap Scripting Engine (NSE) allows users to write simple scripts to automate a wide variety of tasks.

# Scan using default safe scripts

nmap -sV -sC 192.168.0.1
# Get help for a script

nmap --script-help=ssl-heartbleed
# Scan using a specific script

nmap -sV -p 443 –script=ssl-heartbleed.nse 192.168.0.1
# Scan with a set of scripts

nmap -sV --script=smb* 192.168.0.1
# Update script database

nmap --script-updatedb

# Some useful NSE scripts
# Scan for UDP DDOS reflectors:

nmap –sU –A –PN –n –pU:19,53,123,161 –script=ntp-monlist,dns-recursion,snmp-sysdescr 192.168.0.0/24
# This script will scan a target list for systems with open UDP services that allow UDP reflector attack.

# Gather page titles from HTTP servers
nmap --script=http-title 192.168.0.0/24

# Get HTTP headers of web services

nmap --script=http-headers 192.168.0.0/24

# Find web apps from known paths

nmap --script=http-enum 192.168.0.0/24
# Find Information about IP address

nmap --script=asn-query,whois,ip-geolocation-maxmind 192.168.0.0/24
# Find exposed Netbios servers

nmap -sU --script nbstat.nse -p 137 192.168.0.1
# Attempts to pull a zone file (AXFR) from a DNS server:

nmap --script dns-zonetransfer.nse --script-args dns-zonetransfer.domain=<domain> -p53 192.168.0.1
# Retrieve robots.txt files from discovered web servers:

nmap --script http-robots.txt 192.168.0.0/24
# Try to guess valid samba’s username and password combinations using brute force:

nmap --script smb-brute.nse -p445 192.168.0.0/24
