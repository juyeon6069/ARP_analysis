# ARP_analysis

![Example Network](https://user-images.githubusercontent.com/97582404/203703224-abfc92fb-e34e-4a2a-87f1-f22add6e3a5a.png)

<ARP Examples - source from https://www.fortinet.com/resources/cyberglossary/what-is-arp>

ARP(Address Resolution Protocol) is a mechanism to match Mac(media access control) address in the physical layer(L2) and IP(Internet protocol) address the logical layer(L3). This is used to find the MAC address of other person for data communication.


# Code_Analysis

![Lab3_arp_packet](https://user-images.githubusercontent.com/97582404/203704065-185786a8-1981-4301-bab6-42910ba0bfa4.png)

<Internet Protocol (IPv4) over Ethernet ARP packet - source from https://en.wikipedia.org/wiki/Address_Resolution_Protocol>

This code contains data based on packets information shown in the WireShark. It has information about Hardware Type, Protocol Type, Hardware Size, Protocol Size, Sender Mac address, Sender IP address, Target Mac address, Target IP address, wheter it is ARP request or reply, and whether it is gratuitous ARP or not. 

First, to pick only ARP packet, packet's ethernet frame (ethernet.Ethernet(packet[1])) should be 0x0806 which is Ethernet frame field. But it is already filtered during capturing on the WireShark, so in this code, did not filter it. 


Because it is allocate on the Ethernet, therefore, it has 1 as a Hardware type. Using .hrd in dpkt.arp module, Hardware type will be found. Next, for the protocol type, it should have ARP value since only ARP packet is captured. It can be also said it iv IPv4(Internet Protocol version 4) type, because it has IP address of the form of IPv4. Its hardware and protocol size is 6 and 4, respectively. These size is determind as the length of the physical address and IP address in byte, and it can be got using dpkt.arp module, hln for hardware size and pln for protocol size 


And then, in dpkt.arp module, using op(operation value) to determine whether arp packet is ARP Request or ARP Reply. If the value is 1, it means ARP Request. And the value is 2, it means ARP Reply. For the ARP request, it has Gratuitous value, therefore to distinguish whether request signal is it or not, put checking the Target Mac adress is broadcast address, ff:ff:ff:ff:ff:ff.  

To find Senders' and Targets' Mac address, also dpkt.arp module, .sha, is used. However, it is not form of hexidecimal, hex() function should be used together. When this is converted it does not have '::', therefore, using for loop to put '::' by 2 digits. 

To find Senders' and Targets' IP address, also dpkt.arp module, .spa, is used.


# Determine_Mac_address_and_IP_address

![Lab3_ScreenCapture](https://user-images.githubusercontent.com/97582404/203699348-31a62462-72db-44d3-a3a1-1d6616d5a4fb.png)

Above image, the second packet is broadcast on the LAN by a comuter which IP address is 172.16.5.98 to determine the MAC address of a computer with an IP, 172.16.0.1. All device receive the Request and one device with corresponding IP address generate a response packet containing its Mac adress and send it. The third packet is Reply packet of above request. It is also found its packet. When comparing their IP address, request packet and reply packets' sender and target part is just switching. Therefore, it can be said that they link each other.


![Lab3_packet](https://user-images.githubusercontent.com/97582404/203701207-21c2551f-eb11-49b4-8560-0c737e006328.png)
![Lab3_control_plane](https://user-images.githubusercontent.com/97582404/203701560-8c0e7191-8977-49dd-8d67-7e4ddf50cdee.png)
![Lab3_cmd](https://user-images.githubusercontent.com/97582404/203710609-d497d517-d9b8-4e3c-aa7b-494defa54379.png)


Because MAC address(looks 12 hexadecimal digits seperated by hyphens every 2 digits) is phsical address, its value does not change. Therefore, Request packet contains my laptop's MAC address and Reply packet contains wifi's MAC address. To find the MAC address, there is 3ways. First, in WireShark, when we click the packet, they give a information about that packet and it also contains the Mac Address. From second way, only my laptop's mac address will be found because it is the way that find from my device. Second way is to find by using Control Panel. Go to Control Panel and go to Network and Sharing Center and find details. Third way is using Command Prompt. Write ipconfig/all and find physical address for finding Mac address. Comparing this 3ways and it can be confirmed that it is MAC address. 

However, IP address(looks mumerical label) is not uniquely identify devices on the network, but specify connections on the network. To find the IP address, similarly, it also can be found in WireShark in same manner. Another way, is find on the taskbar's wifi part. Click the properties and it shows the IPv4 address.
