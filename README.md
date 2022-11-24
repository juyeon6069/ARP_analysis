# ARP_analysis

ARP(Address Resolution Protocol) is a mechanism to match Mac address in the physical layer(L2) and IP address the logical layer(L3). This is used to find the MAC address of other person for data communication.

This code contains data based on packets information shown in the WireShark. It has information about Hardware Type, Protocol Type, Hardware Size, Protocol Size, Sender Mac address, Sender IP address, Target Mac address, Target IP address, wheter it is ARP request or reply, and whether it is gratuitous ARP or not. 

First, to pick only ARP packet, packet's ethernet frame (ethernet.Ethernet(packet[1])) should be 0x0806 which is Ethernet frame field. But it is already filtered during captureing on the WireShark. Therefore, in this code, do not have to filter it. 
And then, in dpkt.arp module, using op(operation value) to determine whether arp packet is ARP Request or ARP Reply. If the value is 1, it means ARP Request. And the value is 2, it means ARP Reply. For the ARP request, it has Gratuitous value, therefore to distinguish with the real request signal, put checking the Mac adress is ff:ff:ff:ff:ff:ff or not.

