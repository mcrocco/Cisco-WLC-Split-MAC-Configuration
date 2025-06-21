# Cisco-WLC-Split-MAC-Configuration
<img src="https://i.imgur.com/BWoLnB2.png"/>

This project demonstrates how to configure a network for a wireless deployment with a wireless LAN controller (WLC) and Lightweight Access Points (LWAP) via a Split-MAC architecture. 

<p>- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - </p>
<p>Environments and Technologies Used:

- Cisco Packet Tracer
- Cisco Multilayer Switches
- Cisco Command Line
- Cisco WLC
- Cisco LWAPs
- OSPF
- WLANs
- CAPWAP Tunnels
- DHCP</p>

<p>- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - </p>
<p>What is the purpose of this project?
 
The purpose is to demonstrate how to deploy wireless networks by configuring a WLC, LWAPs connected to the same network as the WLC, as well as APs on different networks.
 </p>

<p>- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - </p>

<p>This Project will consist of 11 Objectives:
   
1. Connect PC to WLC port for Initial Setup
2. Configure multilayer switch SW1 (VLANs, ip routing, DHCP, etc)
3. Configure ports on SW1 connected to the WLC, PC, & LWAPs
4. Connect back to the GUI of the WLC using PC web browser
5. Configure dynamic interfaces and WLANs on WLC 
6. Confirm that AP has a CAPWAP connection with the WLC and is broadcasting SSIDs 
7. Attempt to connect to a SSID using a mobile device
8. Configure an OSPF Point-to-Point (PTP) network connection between the Administrator building (SW1) and the Finance building (SW2). Advertise all known networks to each other
9. Configure the 172.20.40.0 /24 network on SW2 and VLAN 40 for the attached LWAP
10. Configure DHCP leasing on SW2 for the LWAP in the 172.20.40.0 /24 network
11. Confirm that the LWAP has formed a connection with the WLC back at the 192.168.10.0 /24 network and is broadcasting SSIDs
</p>

<p>The Administrator Building will consists of 3 VLANs: The Management VLAN (10), Corporate VLAN (20), & Guest VLAN (30). VLAN 10 will have a subnet of 192.168.10.0 /24, VLAN 20 will have a subnet of 192.168.20.0 /24, & VLAN 30 will have a subnet of 192.168.30.0 /24</p>



<p><h3>1. Connect PC to WLC port for Initial Setup</h3>

- First, we want to physically connect our PC to the WLC. We will attach an Ethernet copper cable between the PC and Gigabit Port 1. Note that we could also connect to the CLI of the WLC via the console port, but because we are using Packet Tracer we only have the option of accessing the GUI.
- The default IP for the WLC is 192.168.1.1 /28. We will change the configuration on the PC to have an IP of 192.168.1.2 /28 so that we are on the same subnet as the WLC:

<img src="https://i.imgur.com/DoTdAH3.png"/> 
