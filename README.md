# ARP_analysis

ARP(Address Resolution Protocol) is a mechanism to link the physical layer(L2) and the logical layer(L3). This is used to find the MAC address of other person for data communication.

First, to pick only ARP packet, ethernet.Ethernet(packet[1]) should be 0x0806 which is Ethernet frame field. But I already filter during captureing on the WireShark, I did not. 
And then, in dpkt.arp module, using op(operation value) to determine whether arp packet is ARP request or ARP response. If the value is 1, it is 
