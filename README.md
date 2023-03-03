# Forensic-Incident-Investigation-2022
A Linux server was attacked and we were given a copy of the image and asked to investigate it. 

## Overview

On March 22nd 2004, around 7pm EST a Linux server was attacked. We received reports that there might have been a loss of integrity of files and confidential data leaked. Therefore, we were contacted to analyze the situation, and find out more about the possible attack. The affected system’s image had been copied and provided to us for our analysis.

##	Conducting the Analysis:

The Forensic Analysis of the given image file is divided into the following sections:
* Using the VirtualBox shared folder utility to share the physical image with the Kali Workstation.
* Extracting the partitions from the physical image and mounting it to our Workstation.
* Timeline Analysis
* File and Metadata Recovery
* Analyzing recovered files


### Using the VirtualBox shared folder utility to share the physical image with the Kali Workstation
* Mounted the affected Linux image to our Kali Linux forensic kit.


### Extracting the partitions from the physical image and mounting it to our Workstation
* Created and stored md5sum values and mounted the image with read-only, loop, and no exec parameters.

### Timeline Analysis

* Used **log2timeline** tool to create detailed timeline of all events and checked messages under /var/log/messages for the date of attack.
* Found some suspicious keywords and made a keyword list for further analysis.


### File and Metadata Recovery
* Filtered our keywords from the script and fetched relevant results with their byte and block locations.
* Used **The Sleuth Kit** to search for files and metadata.
* fstat gave us block size, blkstat gave us block status and allocation status, and blkcat gave us info about the deleted file.
* Used **foremost** to analyze and recover deleted files and folders.
* Found the inode number for the gzip file's block number, and then used istat command to get statistical info about the inode number.
* Used icat to recover the data using inode number.

### Analyzing Recovered File
* Used out.file and ffind commands to find the original filename.
* Researched more to eventually find out the tar file. 

* Manufacturer name = **Dark Bay Ltd.**, Product name = **Hacker's Handbook**


## Detailed Analysis of the Rootkit

The rootkit extracts to /last folder, which has total 23 files, out of which 13 are executables. The follow-ing is a list of the executables:
* Cleaner: Cleans logs and checks for the logs containing various file types, mostly .tar, .gz, etc.
* Ifconfig: This file replaces the original ifconfig file and doesn’t have Promiscuos mode set.
* Install: This is the main script which handled everything in the attack from setting up di-rectories to copying information.
* last.cgi : Possibly a trojan
* Linsniffer: Sniffs for passwords redirected to tcp.log
* Logclear: Kills the linsniffer process, removes tcp.log file and creates another one, runs linsniffer again to redirect it to tcp.log
* lsattr: Goes to the /dev/ida/.drag-on directory and runs mkxfs and linsniffer
* mkxfs: Probably a trojan
* Netstat: Possibly a trojan
* Ps: Possibly a trojan
* sense : Scans linsniffer results, sorts them and checks for IMAP, Telnet, and FaP pass-words.
* ssh: This one is an ssh file, probably kept for replacing the system one
* top: Possibly a trojan


## Recommendations

A few recommendations have been listed to implement on the system as soon as possible and prevent such attacks in the future:
* Delete Files: The team should immediately disconnect the machine from the internet once booted up, and delete the suspected trojan files mentioned before and kill any processes that might pre-vent their deletion.
* Restore Original Files: The original ipconfig, netstat files should be implemented and ports closed to block such attacks till scans complete.
* System Scans: A full system scan should be carried to remove any unnecessary files.
* Update OpenSSH: The sshd buffer-overflow attack is common with OpenSSH v2.0 and before. Its recommended to get it updated to fix the vulnerability.
* Closing Ports: Unused ports should always be kept closed. Port scanning tools such as nmap should be used,
* Vulnerability Scanners: Vulnerability Scanners like Nessus by Tenable check of weak points throughout the system and let know of any fixes needed.
* IDS/IPS: Intrusion Detection Systems and Intrusion Prevention Systems should be installed to prevent such attacks.
* SSH Security: ssh security should be increased so that any unauthorized user gets limited to none chances of getting into the system.
