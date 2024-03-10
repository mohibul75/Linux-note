Here, In this article I am going to point out how a packet is arrived to the application as known as the jouney of a packet.My main goal is to show how the application get the data.  Everything of this article is my learning from the other's article from internet and tutorial's videos. And Everything is described here in very simplified way.  

The application needs the data of a packet. There are some header in the packet before the data. These headers are required for the checksum to verify the packet is correct and not corrupted. Total three checksum is used here frame checksum, Ip checksum and the tcp checksum. 

The first header of packet is Ethernet header which is also called L2 header. Three main properties of L2 header are Source and destination MAC address and type. Here the type propertise is IP. The second Header is IP header and here is also some propertise. The main propertise among them are length, IP type, header checksum, destination and source IP . For our case the IP type value is TCP. The next header of a packet is TCP header which also consist destination and source port and the checksum. Then the actual data is in a packet.

