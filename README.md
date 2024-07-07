# Setup an iPXE Server

This Ansible Role will set up an iPXE Server that is capable of PXE booting Linux Distributions to a system with Legacy BIOS and UEFI. It is also capable of using the same instance to PXE Boot ESXi onto a UEFI System.

This role will has only been tested to work with IPv4 Implementations.

## Defaults main.yml
This File contains the necessary variables to set up the iPXE Server. Examples are used to show Exepcted Input. All IP Addresses are automatically detected from the Default IPv4 Informaiton on the system, however, they can be changed manually as per the guidance below.

The Directory in which the ISO Files are stored.  
&nbsp; &nbsp; **ISO_DIR:** "/Path/To/ISO/Directory"

The Network that will Host the iPXE Server.  
&nbsp; &nbsp; **NET_SUBNET:** 192.168.1.0

The Network Mask that the iPXE Server will use.  
&nbsp; &nbsp; **NET_NETMASK:** 255.255.255.0

The Broadcast Address.  
&nbsp; &nbsp; **NET_BROADCAST:** 192.168.1.254

The Networks Gateway.  
&nbsp; &nbsp; **NET_ROUTER:** 192.168.1.1

The IP Address of the iPXE Server.  
&nbsp; &nbsp; **SERVER_IP:** 192.168.1.5

The Start and Stop IP Address for the Addresses the DHCP Server will hand out.  
&nbsp; &nbsp; **DHCP_RANGE_START:** 192.168.1.20  
&nbsp; &nbsp; **DHCP_RANGE_STOP:** 192.168.1.30

## Tasks
Tasks are short and perform a specific task to set up the system. 
4-linuxSystems.yml is used to define the specific variables that are passed into 4a-linuxiso.yml. This includes:

A unique varialb used to identify the OS.  
&nbsp; &nbsp; **MENU_DISTRO:** 'RHEL8'

The name of the ISO that will be used to PXE Boot to.  
&nbsp; &nbsp; **ISO_NAME:** 'rhel-8.5-x86_64-dvd.iso'

The name of the Kickstart File.  
&nbsp; &nbsp; **KICKSTART:** "" \# No Kickstart used  
or  
&nbsp; &nbsp; **KICKSTART:** "myKickstart.cfg"

If a Kickstart file is not specified (string of length 0), then the system will boot into the default ISO menu. If a Kickstart file is specified, the ISO in which it is associated should be filled into the ISO_NAME. The Kickstart should be templatized and placed into the templates/ directory, with ".j2" added to the filename. For the above example of "myKickstart.cfg", the template was added as "templates/myKickstart.cfg.j2"

Similar to 4 and 4a is 5 and 5a. These will setup the ESXi systems. The same rules apply as above for these systems.

**main.yml** is where each of the different tasks are called. This can be customized based on the need of the system.

## Templates
These are used to setup the system. Also, each Kickstart file that is being used should be placed into the templates directory to be used during the setup of the iPXE Server.

## setupiPXEServer.yml
This can be used to execute the role. If this role is incorporated into a larger role, then this YAML can be omitted.

## destroyiPXEServer.yml 
This playbook is independent of the role, but can be used to remove the iPXE Server components.
