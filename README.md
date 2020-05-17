# Network Lab Summary

- [Network Lab Summary](#network-lab-summary)
  * [Command Line Modes](#command-line-modes)
  * [Basic Commands](#basic-commands)
    + [Configure the router name](#configure-the-router-name)
    + [Login banner](#login-banner)
    + [prevent translation](#prevent-translation)
    + [Enable password for privileged mode](#enable-password-for-privileged-mode)
    + [Enable encrypted password for privileged mode](#enable-encrypted-password-for-privileged-mode)
    + [Enable console port password](#enable-console-port-password)
    + [Enable virtual teletype](#enable-virtual-teletype)
    + [Ethernet interfaces configuration](#ethernet-interfaces-configuration)
      - [Subnet Mask Table Example](#subnet-mask-table-example)
    + [Save running config to the startup config file](#save-running-config-to-the-startup-config-file)
  * [Static Route Configuration](#static-route-configuration)
    + [Static route](#static-route)
    + [Default static route](#default-static-route)
  * [Dynamic Routing Protocols Configuration](#dynamic-routing-protocols-configuration)
    + [RIP Protocol Configuration](#rip-protocol-configuration)
    + [OSPF Protocol Configuration](#ospf-protocol-configuration)
      - [Wildcard Table Example](#wildcard-table-example)
    + [OSPFv3 Configuration](#ospfv3-configuration)
  * [DHCP Protocol Configuration](#dhcp-protocol-configuration)
    + [Exclude statically assigned addresses](#exclude-statically-assigned-addresses)
    + [DHCP Pool Configuration](#dhcp-pool-configuration)
  * [FTP Server Configuration](#ftp-server-configuration)
    + [Enable FTP on the server](#enable-ftp-on-the-server)
    + [Upload and Download a file to/from the FTP server](#upload-and-download-a-file-to-from-the-ftp-server)
  * [NAT Router Configuration](#nat-router-configuration)
    + [Static NAT](#static-nat)
    + [Dynamic NAT](#dynamic-nat)
    + [PAT](#pat)
    + [Inside or Outside Interfaces](#inside-or-outside-interfaces)
  * [InterVLAN Configuration](#intervlan-configuration)
  * [IPv6 Configuration](#ipv6-configuration)
  * [VLAN Configuration](#vlan-configuration)
    + [Creating VLANs](#creating-vlans)
    + [Configure Ports To Access Mode](#configure-ports-to-access-mode)
    + [Configure Ports To Truck Mode](#configure-ports-to-truck-mode)
  * [InterVLAN Configuration](#intervlan-configuration)
  * [Troubleshooting Commands](#troubleshooting-commands)
    + [Show the routing table of the router](#show-the-routing-table-of-the-router)
    + [Show running Configuration](#show-running-configuration)
    + [Show DHCP Bindings](#show-dhcp-bindings)
    + [Checking for connectivity](#checking-for-connectivity)
    + [OSPFv3 Validation](#ospfv3-validation)
    + [Show defined VLANs](#show-defined-vlans)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Command Line Modes
- User mode
- Privileged mode
- Global configuration mode
  - Interface mode
  - Line mode
  - Router mode

## Basic Commands

### Configure the router name
Router(config)# **hostname** _name_

### Login banner
A login banner is a message that is displayed at login

Router(config)# **banner motd**  #_statment_#  

### prevent translation
Prevent the translation of incorrectly entered commands as though they were hostnames

Router(config)# **no ip domain-lookup**

### Enable password for privileged mode
Router(config)# **enable password** _yourpassword_

### Enable encrypted password for privileged mode
Router(config)# **enable secret** _yourpassword_

### Enable console port password
Router(config)# **line console** _0_  
Router(config-line)# **login**  
Router(config-line)# **password** _yourpassword_  

### Enable virtual teletype
Router(config)# **line vty** _0_ _4_
Router(config-line)# **login**  
Router(config-line)# **password** _yourpassword_

### Ethernet interfaces configuration
Router(config)# **interface** _interface-name_ 

Router(config-if)# **description** _descriptive-text_

Router(config-if)# **ip address** _ip-address_  _subnetmask_  

Router(config-if)# no shutdown  

#### Subnet Mask Table Example
| Prefix | Mask   |
| :---:  | :----: |
| /24    | 0      |
| /25    | 128    |
| /26    | 192    |
| /27    | 224    |
| /28    | 240    |
| /29    | 248    |
| /30    | 252    |

### Save running config to the startup config file
Router# **copy** running-config startup-config

## Static Route Configuration

### Static route
Router(config)# **ip route** _network-address_ _subnet-mask_ _{ ip-address | exit-interface }_

- **_network-address_**: Destination network address of the remote network to be added to the routing table.
- **_subnet-mask_**: Subnet mask of the remote network to be added to the routing table.
- **_ip-address_**: next-hop router's ip address
- **_exit-interface_**: Outgoing interface that would be used in forwarding packets to the destination network.

### Default static route
Router(config)# **ip route 0.0.0.0 0.0.0.0** _{ ip-address | exit-interface }_

## Dynamic Routing Protocols Configuration

### RIP Protocol Configuration
Router(config)# **router rip**  
Router(config-router)# **version** _2_  
Router(config-router)# **no auto-summary**  

Once you are in routing configuration mode, enter the classless network address for each directly connected network, using the network command.  

Router(config-router)# **network** _network-address_  

This command prevents sending RIP updates to devices that will not use these updates.
(and we can use it in OSPF)

Router(config-router)# **passive-interface** _interface-name_  

### OSPF Protocol Configuration
Router(config)# **router ospf** _process-id_  
Router(config-router)# **network** _network-address_ _wildcard-mask_ **area** _0_  

**wildcard-mask** = 255.255.255.255 – **subnet-mask**  

#### Wildcard Table Example
| Prefix | Wildcard |
| :---:  | :----:   |
| /24    | 255      |
| /25    | 127      |
| /26    | 63       |
| /27    | 31       |
| /28    | 15       |
| /29    | 7        |
| /30    | 3        |

### OSPFv3 Configuration
Router(config)# **ipv6 router ospf** _process-id_  
Router(config-rtr)# **router-id** _router-id_  
Router(config-rtr)# **passive-interface** _interface-name_  

For each interface perform this command:  
Router(config-if)# **ipv6 ospf** _process-id_ **area** _0_  

**note**: The router ID consists of 32 bits and should be different for each router.  

## DHCP Protocol Configuration
### Exclude statically assigned addresses
Router(config)# **ip dhcp excluded-address** first-ip last-ip  

For example to execlude the first 7 ip addresses on 192.168.20.0/24 network:  
Router(config)# **ip dhcp excluded-address** 192.168.20.1 192.168.20.7  

### DHCP Pool Configuration
Router(config)# **ip dhcp pool** _pool-name_  
Router(dhcp-config)# **network** _network-address_ _subnet-mask_  
Router(dhcp-config)# **dns-server** _ip-address_  
Router(dhcp-config)# **default-router** _ip-address_  

## FTP Server Configuration
### Enable FTP on the server
1) Click **Server** > **Services tab** > **FTP**
2) Click **On** to enable FTP service.
3) In **User Setup**, create the user accounts.

### Upload and Download a file to/from the FTP server
1) **PC** > **Desktop tab** > **Text Editor**
2) Write any text you want, then click **File** > **Save**
3) Open the Command prompt of the **PC** and type `dir` command, you will find your file in the list.
4) On Command Prompt of **PC** type `ftp server-address`

To list files that are on the server  
**ftp> dir**  

To upload a file to the server  
**ftp> put** _file-name_  

To download a file from the server  
**ftp> get** _file-name_  

To quit the FTP session  
**ftp> quit**  

## NAT Router Configuration
### Static NAT
Statically map a public IP address to a private IP address using:  

Router(config)# **ip nat inside source static** _private-address public-address_  

### Dynamic NAT

Step 1 - Define the pool of addresses that will be used for translation

Router(config)# **ip nat pool** _pool-name first-IP second-IP_ **netmask** _subnet-mask_

Step 2 - Configure a standard ACL to identify (permit) only those addresses that are to be translated.

Router(config)# **access-list** _list-identifier-number_ **permit** _IP-address wildcard_

Step 3 - Bind the ACL to the pool, using the ip nat inside source list command.

Router(config)# **ip inside source list** _list-identifier-number_ **pool** _pool-name_

### PAT



### Inside or Outside Interfaces

Before NAT can work, you must specify which interfaces are inside and which interfaces are outside.  

Router(config)# **interface** _interface-name_  
Router(config-if)# **ip nat outside**  

Router(config)# **interface** _interface-name_  
Router(config-if)# **ip nat inside**  

## IPv6 Configuration
Router(config)# **ipv6 unicast-routing**  

Router(config)# **interface** _interface-name_  

Router(config-if)# **ipv6 address** _2001:DB8:1:1::1/64_  
Router(config-if)# **ipv6 address** _FE80::1_ **link-local**  

Router(config-if)# **no shutdown**  

## VLAN Configuration

### Creating VLANs
Switch(config)# **vlan** _vlan-id_  
Switch(config-vlan)# **name** _vlan-name_  

### Configure Ports To Access Mode
Switch(config)# **interface** _interface-name_  
Switch(config-if)# **switchport mode access**  
Switch(config-if)# **switchport access vlan** _vlan-id_  

### Configure Ports To Truck Mode
Switch(config)# **interface** _interface-name_  
Switch(config-if)# **switchport mode trunk**  
Switch(config-if)# **switchport trunk allowed vlan** _vlan-list_  

**vlan-list**: comma separated list of vlan ids  

## InterVLAN Configuration

Making a sub-interface G0/1 is the interface name and 10 is the vlan id  
Router(config)# **interface** _G0/1.10_  
Router(config-subif)# **encapsulation dot1Q** _10_  
Router(config-subif)# **ip address** _ip-address_ _subnet-mask_  

## Troubleshooting Commands

### Show the routing table of the router
Router# **show ip route**  

### Show running Configuration
Router# **show running-config**

### Show DHCP Bindings
Router# **show ip dhcp binding**  

### Checking for connectivity
Works in any device  
Router# **ping** _ip-address_

### OSPFv3 Validation
Router# **show ipv6 route**  
Router# **show ipv6 protocols**

### Show defined VLANs
Switch# **show vlan**  
