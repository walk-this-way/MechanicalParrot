<!-- vscode-markdown-toc -->
	* 1. [Introduction](#Introduction)
	* 2. [Hardware](#Hardware)
	* 3. [Physical Networking](#PhysicalNetworking)
	* 4. [Logical VM Networks](#LogicalVMNetworks)
		* 4.1. [1. FAK firewall configuration](#FAKfirewallconfiguration)
		* 4.2. [2. VM networks](#VMnetworks)
	* 5. [How to connect to kit remotely](#Howtoconnecttokitremotely)
	* 6. [Proposed software list](#Proposedsoftwarelist)
		* 6.1. [1. What we used at VALEX according to memory](#WhatweusedatVALEXaccordingtomemory)
		* 6.2. [2. Full software list](#Fullsoftwarelist)
	* 7. [Team SOPs](#TeamSOPs)
		* 7.1. [Harden](#Harden)
		* 7.2. [Clear](#Clear)
		* 7.3. [Hunt](#Hunt)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc --># MechanicalParrot



###  1. <a name='Introduction'></a>Introduction

I couldn't copy anything out of MS teams with my permissions, but here is a presentation from 2021.

[2021 pdf presentation link to sharepoint in MS Teams](https://armyeitaas.sharepoint-mil.us/:b:/r/teams/170CPTGeneral/Shared%20Documents/General/Fly-Away%20Kit/FAK_Presentation.pdf?csf=1&web=1&e=7T6c35)


Key people:

| Name     | Role                  |
|----------|-----------------------|
| Person A | Project Manager       |
| Person B | Networking            |
| Person C | Software manager      |
| Person D | Documentation manager |
| Person E | Engineer              |


###  2. <a name='Hardware'></a>Hardware

|             | Dell PowerEdge R6515                             | QNAP TS-883XU-RP             | Cisco C9200L-24PXG           | DualComm ETAP-5203 |
|-------------|--------------------------------------------------|------------------------------|------------------------------|--------------------|
| Device type | ESXI VM host                                     | Storage                      | L3 switch                    | Network TAP        |
| Compute     | AMD EPYC 7702 64C                                |                              |                              |                    |
| Memory      | 384GB DDR4 RDIMM                                 | 32GB DDR4 UDIMM              |                              |                    |
| Storage     | 2x 1.6TB SSD, 6x 2.4TB 10K HDD, 2x M2 BOSS RAID1 | 3x 3.84 TB SSD, 5x 10TB HDD  |                              |                    |
| Network     | 2x 10GBASE-T, 2x 25GbE SFP28                     | 2x 10GBASE-T, 2x 25GbE SFP28 | 16x GbE / 8x 10GbE, 2x 25GbE | 2x GbE             |
|             |                                                  |                              |                              |                    |
|             |                                                  |                              |                              |                    |

###  3. <a name='PhysicalNetworking'></a>Physical Networking

![Physical network map](https://gitlab.com/170FAK/mechanicalparrot/uploads/c4c7f190a8b354502767d12a1406e015/FAK_Diagram.svg "Physical network map")

###  4. <a name='LogicalVMNetworks'></a>Logical VM Networks

####  4.1. <a name='FAKfirewallconfiguration'></a>1. FAK firewall configuration
[See full documentation on wiki page](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/FAK-FireWall-Rules-and-Configuration-Wiki)
[![FAK firewall configuration](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/uploads/6d064517edec075f6358191d6622fa99/image.png "FAK firewall configuration")](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/FAK-FireWall-Rules-and-Configuration-Wiki)


####  4.2. <a name='VMnetworks'></a>2. VM networks

| VLAN name              | IP Range      | Gateway    | DHCP               |
|------------------------|---------------|------------|--------------------|
| VLAN 25 - WAN          | DHCP          | DHCP       |                    |
| VLAN 50 - DMZ          | 10.0.50.0/24  | 10.0.50.1  | None               |
| VLAN 100 - Workstation | 10.0.100.0/24 | 10.0.100.1 | 10.0.100.100 - 199 |
| VLAN 110 - Forensics   | 10.0.110.0/24 | 10.0.110.1 | 10.0.110.100 - 199 |
| VLAN 120 - CTE         | 10.0.120.0/24 | 10.0.120.1 | 10.0.120.100 - 199 |
| VLAN 130 - Malware     | 10.0.130.0/24 | 10.0.130.1 | 10.0.130.100 - 199 |
| VLAN 240 - Storage     | 10.0.240.0/24 | 10.0.240.1 | None               |
| VLAN 250 - Server      | 10.0.250.0/24 | 10.0.250.1 | 10.0.250.100 - 199 |
| VLAN 254 - Management  | 10.0.254.0/24 | 10.0.254.1 | None               |



###  5. <a name='Howtoconnecttokitremotely'></a>How to connect to kit remotely

[![VPN connection](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/uploads/b910fc7e49761873e13dc47827a8b768/FAK_VPN_logical.png "VPN connection")](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/VPN-Connection-Wiki)

[VPN Connection Wiki](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/VPN-Connection-Wiki)
[SSH Connection Wiki](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/SSH-Connection-Wiki)
[Google Authenticator Wiki](https://gitlab.com/170FAK/mechanicalparrot/-/wikis/Google-Authenticator-Wiki)


###  6. <a name='Proposedsoftwarelist'></a>Proposed software list

####  6.1. <a name='WhatweusedatVALEXaccordingtomemory'></a>1. What we used at VALEX according to memory

| Software     | Teams             | Used/Desired | Pain points                                                                                   |
|--------------|-------------------|--------------|-----------------------------------------------------------------------------------------------|
| Elastic      | Hunt              | Used         |                                                                                               |
| Endgame      | Hunt              | Used         |                                                                                               |
| Chat         | Hunt/Harden/Clear | Used         |                                                                                               |
| Ticketing    |                   | Desired      |                                                                                               |
| File sharing | Hunt/Harden/Clear | Used         | How to share and have multiple people edit network diagrams and spreadsheets at the same time |
| VMs          | Hunt/Harden/Clear | Used         | Weren't connected into the valex, only used for rehearsal drills                              |
| Networking   |                   |              | Had bad internet connection, bad connection into VALEX, and VALEX systems were slow.          |

####  6.2. <a name='Fullsoftwarelist'></a>2. Full software list

| Name                | cost   |
|---------------------|--------|
| ACAS/NESSUS         | govt   |
| ArcSight/OSSIM      |        |
| Blue Coat           |        |
| Blue Scope          |        |
| Bro                 |        |
| Burpsuite Pro       |        |
| Dagger              | govt   |
| Dark Ether          | govt   |
| dig                 | native |
| elastic search      |        |
| ELSA for bro        |        |
| EnCase              |        |
| ePO                 |        |
| Galaxy / Pointlist  | govt   |
| gpg                 |        |
| Grass Marlin        | govt   |
| IDA free            |        |
| Ghidra              |        |
| HBSS                | govt   |
| iptables            | native |
| applocker           | native |
| kali                |        |
| LogParser           |        |
| Maltego             |        |
| Metasploit          |        |
| Mimikatz            |        |
| Multiverse          | govt   |
| Nagios              |        |
| netcat              |        |
| NetRaptor           |        |
| netsniff            |        |
| netstat             |        |
| nmap                |        |
| Norman Sandbox      |        |
| Notepad++ / Sublime |        |
| nslookup            |        |
| OllyDbg / x64dbg    |        |
| openssh             |        |
| openssl             |        |
| openvas             |        |
| OTRS                |        |
| Palo Alto firewall  |        |
| powershell          |        |
| PS Tools            |        |
| Putty               |        |
| qtip                |        |
| RAT                 |        |
| Rkhunter            |        |
| rsyslog             |        |
| SCCIM               |        |
| Security Onion      |        |
| Snort               |        |
| Snorby              |        |
| Sophia              |        |
| Solarwinds***       |        |
| Splunk              |        |
| Squil               |        |
| Sysinternals Suite  |        |
| Process Hacker      |        |
| tcpdump             |        |
| volatility          |        |
| wireshark           |        |


###  7. <a name='TeamSOPs'></a>Team SOPs

####  7.1. <a name='Harden'></a>Harden

| Priority/Phase | Task                                                                        |
|----------------|-----------------------------------------------------------------------------|
| 1              | Analyze mission                                                             |
| 1              | Get initial domain access from customer                                     |
| 2              | Install EDR                                                                 |
| 2              | Reset/audit all credentials                                                 |
| 3              | Restrict firewalls to meet minimum requirements                             |
| 4              | Default harden GPO configuration (IE disable SMBv1)                         |
| 5              | Assess site specific vulns, OS, system, software versions and configuration |
| 6              | Harden site-specific vulns based on assessment.                             |


####  7.2. <a name='Clear'></a>Clear

(TODO)

####  7.3. <a name='Hunt'></a>Hunt

(TODO)