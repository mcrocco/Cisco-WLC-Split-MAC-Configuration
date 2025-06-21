# Cisco-WLC-Split-MAC-Configuration
<img src="https://i.imgur.com/oTVjSX8.png"/>

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
8. Configure the 172.20.40.0 /24 network on SW2 and VLAN 40 for the attached LWAP
9. Configure an OSPF Point-to-Point (PTP) network connection between the Administrator building (SW1) and the Finance building (SW2). Advertise all known networks to each other
10. Configure DHCP leasing on SW2 for the LWAP in the 172.20.40.0 /24 network
11. Confirm that the LWAP has formed a connection with the WLC back at the 192.168.10.0 /24 network and is broadcasting SSIDs
</p>

<p>The Administrator Building will consists of 3 VLANs: The Management VLAN (10), Corporate VLAN (20), & Guest VLAN (30). VLAN 10 will have a subnet of 192.168.10.0 /24, VLAN 20 will have a subnet of 192.168.20.0 /24, & VLAN 30 will have a subnet of 192.168.30.0 /24</p>



<p><h3>1. Connect PC to WLC port for Initial Setup</h3>

- First, we want to physically connect our PC to the WLC. We will attach an Ethernet copper cable between the PC and Gigabit Port 1. Note that we could also connect to the CLI of the WLC via the console port, but because we are using Packet Tracer we only have the option of accessing the GUI.
- The default IP for the WLC is 192.168.1.1 /28. We will change the configuration on the PC to have an IP of 192.168.1.2 /28 so that we are on the same subnet as the WLC:

<img src="https://i.imgur.com/DoTdAH3.png"/> 

- Then, ping 192.168.1.1 to confirm that each device can reach each other:

<img src="https://i.imgur.com/zyvFVfc.png"/> 

- Head to the web browser and type in the IP address 192.168.1.1. After a little bit, the welcome page for the WLC GUI will appear:

<img src="https://i.imgur.com/8ckjpEH.png"/> 

-  We will configure the initial admin account with a username of “admin” and password of “Cisco123”.
-  The GUI will then ask for a new System Name, management IP address, default gateway, and management VLAN ID. We will use 192.168.10.50 /24 for the management IP of the WLC controller, and map it to VLAN 10, the management VLAN. The System name will be WLC1, with a default gateway of 192.168.10.1, which will be the SVI IP of SW1 once we configure it:

<img src="https://i.imgur.com/Tsbgu1J.png"/> 

- The WLC will then ask us to configure at least one wireless network. We will configure the Corporate network, with WPA2 PSK as the security method, with a passphrase of Cisco123:

<img src="https://i.imgur.com/Jq5hSfC.png"/> 

-  Note that it will be mapped to the management VLAN at the current configuration. We will edit this later to map it to our Corporate VLAN 20. 
- After this, select "Next" then "Apply", in which the WLC will reboot with its newly configured settings
</p>

<p><h3>2. Configure multilayer switch SW1 (VLANs, ip routing, DHCP, etc)</h3>

- SW1 will be performing all of the routing in this network, as well as act as a DHCP server for each VLAN. In this topology, there will be 3 VLANs: Management (10), Corporate (20), & Guest (30). Lets start by configuring the VLANs on SW1:

<img src="https://i.imgur.com/QnkXe93.png"/> 


- We also want to configure an SVI for each VLAN so that the switch can perform inter-vlan routing. We will configure a .1 IP for each SVI, since it will be the gateway for all VLANs:


<img src="https://i.imgur.com/BMDRzyw.png"/> 


- Even with the configuration of SVIs, this switch is still only operating in Layer 2. We need to enter the command “ip routing” to enable Layer 3 capabilities:


<img src="https://i.imgur.com/mauO233.png"/> 


- Next, we will configure the SW1 to be a DHCP server for each VLAN. We will configure the DHCP server to share information on the default gateway (the SVI of the VLAN), as well as option 43. Option 43 isn’t necessary for the LWAPs connected to this same switch, but it necessary for other APs in different networks to be able to know the IP of the WLC to connect to:


<img src="https://i.imgur.com/d5telcI.png"/> 

</p>

<p><h3>Configure ports on SW1 connected to the WLC, PC, & LWAPs</h3>

- We now need to configure the ports of SW1 that are connected to the LWAPs, PC, and the WLC. The port to the WLC will be a trunk port, as every VLAN will need to be transported between the WLC and SW1:

<img src="https://i.imgur.com/c8sk4xA.png"/> 

- The port for the PC will be in the management vlan 10, so that we can connect back to the WLC management GUI to further configure it. Therefore, we will configure it as an access port.
- The port for AP1 and AP2  will also be configured as an access port for VLAN 10, as it only needs to be able to communicate with the WLC. If it receives wireless data traffic from SSIDs for VLANs 20 & 30, the AP will use CAPWAP to encapsulate the traffic to the WLC. The WLC will then deencapsulate the CAPWAP frame, and forward the traffic out of its trunk port to the appropriate destination/VLAN. This is the purpose of Split MAC architecture:

<img src="https://i.imgur.com/Z5Uivfu.png"/> 

</p>

<p><h3>4. Connect back to the GUI of the WLC using PC web browser</h3>

- Now we can go back on the PC browser to access the WLC GUI. Type in “192.168.10.50” into the browser, and login using the credentials we made earlier. Remember to change the PC IP address settings to DHCP so that it can receive the correct IP address in the same subnet as the WLC:

<img src="https://i.imgur.com/5w66GLU.png"/> 

</p>

<p><h3>Configure dynamic interfaces and WLANs on WLC</h3>

- Once logged in to the GUI, we need to configure the dynamic interfaces. Dynamic interfaces are virtual interfaces which will map the WLAN to the wired VLAN. When the WLC receives traffic from a specific WLAN, it will use this dynamic interface to receive the traffic, and vice versa for sending the traffic out for a specific VLAN. To configure the dynamic interfaces, head to Controller > Interfaces
- From the Interfaces window, you should already see the management interface that we initially configured. In the top right of the window, select the "New" button to create the Corporate dynamic interface:

<img src="https://i.imgur.com/eeHm22a.png"/> 

- From here, we can give it a name of Corporate, give the interface an IP address, associate it with a VLAN ID of 20, configure the gateway of the SVI, and the DHCP server IP, which is also the SVI. We will map this to physical port 1, as this is the physical connection we have to SW1:

<img src="https://i.imgur.com/31p6Kmo.png"/> 
<img src="https://i.imgur.com/q6M8ryH.png"/> 

- After selecting the Apply button, go back to the Interfaces window and you should see the new dynamic interface. We will now do the same for the Guest VLAN (30):

<img src="https://i.imgur.com/VWTs9Ht.png"/> 
<img src="https://i.imgur.com/pudI87f.png"/> 

- Now, we need to configure the WLANs, which will be the SSIDs that are broadcasted wirelessly via the LWAPs. These will be assigned a dynamic interface. On the GUI, select the WLANs tab. Before we create the Guest WLAN, we need to edit the Corporate WLAN we created initially so that it maps to the correct dynamic interface. Select the WLAN ID to edit:

<img src="https://i.imgur.com/OWx7pXm.png"/> 

- From here, change the interface/interface group drop down menu from “management” to “Corporate” and select the Apply button.
- To create a new WLAN, select the Go button next to the Create New drop down menu 

<img src="https://i.imgur.com/0YZ1vzc.png"/> 

- Name the Profile Name and WLAN SSID as Guest, and select the checkbox next to Enabled to enable the WLAN. Change the interface/interface group to our newly created dynamic interface Guest:

<img src="https://i.imgur.com/yZqS0f6.png"/> 

- Select the Security tab, and in the Layer 2 Security drop down menu select the WPA + WPA2. For the WPA + WPA2 Parameters, select AES as the encryption. For Authentication Key Management, select PSK and type in Cisco123 as the pre-shared key:

<img src="https://i.imgur.com/lJLLK0t.png"/> 

- This means that when a wireless client attempts to connect to the SSID, it will need to enter this same key to be granted access to the WLAN/VLAN. Select Apply to finish creating the WLAN.
- We should now see both WLANs enabled on the WLC GUI:

<img src="https://i.imgur.com/i9rEFrL.png"/>


</p>

<p><h3>6. Confirm that AP1 & AP2 has a CAPWAP connection with the WLC and is broadcasting SSIDs</h3>

- We should now see that AP1 & AP2 has formed a CAPWAP connection with the WLC. In the GUI, head to the Wireless tab:

<img src="https://i.imgur.com/F4DoeDM.png"/>

- We can also see on AP1 the IP address it has received from SW1, as well as the SSIDs it is broadcasting:

<img src="https://i.imgur.com/ItE8Dvp.png"/>

</p>

<p><h3>7. Attempt to connect to a SSID using a mobile device</h3>

- We will now attempt to wirelessly connect to an AP using a mobile device. In the mobile device configuration, enter the SSID of Guest, change the Authentication to WPA2-PSK, and the PSK of Cisco123:

<img src="https://i.imgur.com/2lHmC6E.png"/>

- Note that in a real scenario, we wouldn’t have to manually input the SSID or Authentication method. The SSID should automatically appear as the AP is broadcasting it out via its configured 802.11 beacons.
- We can now see that the mobile device has received an IP address:

<img src="https://i.imgur.com/SL9YKuT.png"/>

- Note that it has received an IP address from the management VLAN. This is a known bug in Packet Tracer, not a misconfiguration. In a real scenario, it would’ve received an IP address from the 192.168.20.0 /24 subnet.

</p>

<p><h3>8. Configure the 172.20.40.0 /24 network on SW2 and VLAN 40 for the attached LWAP</h3>

- SW2 needs to be configured for Layer 3, with a SVI for vlan 40. VLAN 40 is the vlan that the AP will connect to:

<img src="https://i.imgur.com/rgC0inu.png"/>

</p>

<p><h3>9. Configure an OSPF Point-to-Point (PTP) network connection between the Administrator building (SW1) and the Finance building (SW2). Advertise all known networks to each other</h3>

- To connect both multilayer switches SW1 & SW2 together from separate networks, we will configure an OSPF PTP network. OSPF will allow both switches to dynamically advertise their networks to each other, so that the WLC can form an CAPWAP tunnel to the LWAP connected to the Finance network. The PTP network will be 10.0.0.0 /30. We will make each interface a Layer 3 interface with the “no switch port” command, with the following OSPF commands:

<img src="https://i.imgur.com/5m3zS5U.png"/>

- For SW2, advertise the 172.20.40.0 /24 network to SW1. On SW1, advertise the 192.168.10.0 /24, 192.168.20.0 /24, & 192.168.30.0 /24 network to SW2. Make sure to also enable OSPF on the interfaces that are in the 10.0.0.0 /24 network. 

<img src="https://i.imgur.com/KWWVfCa.png"/>

- Verify that each switch has all networks in their routing table:

<img src="https://i.imgur.com/aPhqhkF.png"/>

<img src="https://i.imgur.com/91M1AeX.png"/>

</p>

<p><h3>10. Attempt to connect to a SSID using a mobile device</h3>

- SW2 will also be a DHCP server for its network. Option 43 will be configured for the 192.168.10.50 IP, so that the AP knows how to connect to the WLC:

<img src="https://i.imgur.com/9mfRuRU.png"/>

</p>

<p><h3>11. Confirm that the LWAP has formed a connection with the WLC back at the 192.168.10.0 /24 network and is broadcasting SSIDs</h3>

- Configure port fa0/2 as an access port for VLAN 40. With everything configured for the 172.20.40.0 network, the AP should now form a CAPWAP connection back to the WLC, and should be broadcasting the Corporate and Guest SSIDs:

<img src="https://i.imgur.com/u5u0BL2.png"/>

