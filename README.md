# CTFRecon.py
 Basic reconnaissance automation tool for HackTheBox machines.

## Description

 Information gathering (reconnaissance) is a crucial, yet an exhausting process due to its repetitiveness. While participating in HackTheBox seasonal events, I created a tool in Python that automates new CTF initialization / basic reconnaissance process. The source code is modification-friendly; if required, you can easily adjust the current scan options, integrate new tools or custom-built vulnerability scanners to expand functionality according to your needs.

## Features

 The tool has two run modes supplied by the user as arguments: `new` and `delete` and is required to run with SUDO in order to make changes in */etc/hosts* file as well as to simplify possible integration of certain tools that require SUDO privileges.

 `new` run mode:

 ```python
 sudo python3 CTFRecon.py new -ip 10.129.107.139 -n test
 ```
 * Requests target IP (`-ip`) and CTF name (`-n`) as arguments.
 * Creates a */home/username/CTFs/name* directory, requests a unique directory name until it's given.
 * Adds a new "IP-hostname.htb" entry to */etc/hosts* file for hostname resolution (since DNS server is not available).
 * Checks if a webserver is present on the target (on common HTTP ports 80 and 8080). If yes, runs 3 types of scanning as parallel processes with the following tools (If a webserver is not present, only runs **nmap** scan):
   * Port scanning: **nmap**
   * Directory and file enumeration: **dirsearch**
   * Subdomain enumeration: **ffuf**
 * Awaits until all processes complete, saves enumeration results as separate files in the created directory.
 **NOTE**: ffuf is running with `-or` argument, so the output file will only be created if results are present.
 * Notifies the user of successful completion.

 `delete` run mode:
 ```python
 sudo python3 CTFRecon.py delete -n test
 ```

 * Checks if a CTF directory exists, and in such case deletes it along with its contents as well as a corresponding entry from */etc/hosts* file.
