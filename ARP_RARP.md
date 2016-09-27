# ARP and RARP
Address Resolution Protocol (ARP) and Reverse Address Resolution Protocol (RARP) are two protocols that reside at the **intersection of layer 2 (data link layer) and layer 3 (network layer)** of the OSI model. Both of them play a critical role in address resolution.

## Address Resolution Protocol (ARP)
### Why is there a need for address resolution?

This is because internetwork communication happens at layer 3 using network addresses, but the actual transmission of data occurs at layer 2 using data link addresses. Hence, it is important to have some means to resolve data link layer address (Layer 2) to network layer addresses (Layer 3).

Layer 2 addresses are for **local transmission between hardware devices** that are directly connected and can comunicate directly e.g. in LANs, WLANs or WANs. 

Layer 3 addresses are for **internetworked tranmission of virtual networks** that are "indirectly" connected (e.g. they are not in the same physical network).

----------

###  ARP Methods

1. Direct Mapping
2. Dynamic Address Resolution


#### Direct Mapping
This uses a simple algorithm to **convert one to the other**. When we have one address, we already have the other. For example, IP address 222.101.33.1 can be mapped to ARCNet address #1 and 222.101.33.4 can be mapped to ARCNet address #4. Hence, only the final 8 bits of the IP address is used to derive the ARCNet address.

**Problems**: This however creates a problem because the IP addresses are 32-bits long and MAC addresses at the data link layer are 48-bits long.  The address space of the MAC addresses is much larger in size than the address space of the IP addresses. This will result in mapping duplication and rigidity. 

#### Dynamic Address Resolution
This involves working out the address post **broadcasting to multiple devices on the network**. Only the corresponding target recipient device will send back a response to acknowledge its identity. This approach is the approach used as of today.

To improve efficiency and viability of this approach, it has to be coupled with the following two mechanism: 

- **caching**: reduce the repeated need for sending message to perform address resolution when the target recipient is the same.

- **cross resolution**: once the sender resolve the target address, the target also adds a record to its cache of the sender address. 

----------

### ARP Message Type
The sender wants to send a datagram across, therefore the sender sends an **ARP Request Message** to the target. The target acknowledges and responses with a **ARP Reply Message** to the sender. 

The ARP mesages will contain the following information:

- **Sender Hardware Address**: layer 2 address of the sender of the ARP message

- **Sender Protocol Address**: layer 3 address of the sender of the ARP message

- **Target Hardware Address**: layer 2 address of the target of the ARP message

- **Target Protocol Address**: layer 3 address of the target of the ARP message

----------

### ARP Message Format
ARP  Messages are 28 bytes consisting of:

- **Sender Hardware Address** (48-bits/6 bytes)

- **Sender Protocol Address** (32-bits/4 bytes)

- **Target Hardware Address** (48-bits/6 bytes)

- **Target Protocol  Address** (32-bits/4 bytes)

- **Operational Code** (2 bytes): 1/2 for regular ARP, 3/4 for RARP 

- **Hardware Type** (2 bytes): type of hardware used for local networking transmission of ARP message e.g. Ethernet, IEEE 802, ARCNeT.

- **Protocol Type** (2 bytes): type of protocol type at layer 3 addressing e.g. IPv4.

- **Hardware Address Length** (1 byte): how long the hardware address is in the message e.g.  IEEE 802 MAC addresses will have 6 as value.

- **Protocol Address Length** (1 byte): how long layer 3 address is in the message e.g. IPV4 will have 4 as value.
 

----------

### ARP General Operation
1. Source device **checks cache** to see if there is a resolution for the target device.

2. If not, source device will **generate an ARP Request Message** with the following information: Sender Hardware Address, Sender Protocol Address, Target Protocol Address (no Target Hardware Address since it doesn't know at this stage ).

3. Source device **broadcast ARP Request Message** to members of its local network. 

4. The target device **generates an ARP Reply Message** with the Sender Hardware Address and Sender Protocol Address as the Target Hardware Address and the Target Protocol Address. It then fills in its layer 2 address as the Sender Hardware Address and its layer 3 address as the Sender  Protocol Address. 

5. Target device **updates ARP Cache** with entry about the sender hardware address and IP address that was sent through the initial ARP Request Message.

6. Target device **sends ARP Reply Message** in a unicast approach to the initial message sender.

7. Source Device **processes ARP Reply Message** and stores Sender Hardware Address as layer 2 address of the destination to use it for sending the datagram.

8. Source device **update ARP Cache** with Sender Hardware Address and Sender Protocol Address for future usage of tranmission.

----------
### ARP Caching Mechanism

1.  **Static ARP Cache Entries**: manually added to the cache table for the device and kept at a permanent basis using arp software utility. This requires housekeeping if any of the cache details change over time. 

2.  **Dynamic ARP Cache Entries**: derived through past ARP resolution and kept in cache for a period of time and then removed after 10-20 minutes. This timeout is crucial as there could be changes in device hardware, device IP address or device removal.


----------
### Proxy ARP

This is used when the message sender and message recipient are situated in two separate physical network. In this case, it cannot use pass hardware layer broadcasts. Hence, the **router** sits in as the "recipient" that relays the message to the actual recipient. 

----------
### TCP/IP Address Resolution for Multicast Addresses
Multicasting a datagram to multiple recipients/addresses, require knowing the maping between the network layer multicast groups and data link layer multicast groups. The most common scheme at the data link layer is IEEE 802 addressing system that have 48-bits Ethernet multicast hardware address split into two blocks of 24-bits. 

The upper 24-bits block is called the organizationally unique identifier (OUI) with different values assigned to individual organizations, the lower 24 bits are used for specific devices.The 25th bit is always zero, thus leaving 23 bits of the multicase hardware address for mapping to the 32-bits multicast IP address. Out of the 32-bit multicast IP address, only 28-bits are unique bits, thus this leaves a small group of 32 different IP multicast addresses having the same 48-bits Ethernet multicast hardware addresses. 


----------

### TCP/IP Address Resolution for IPV6
It uses a different protocol known as **Neighbour Discovery (ND)** Protocol, which is a combination of ARP with Internet  Control Message Protocol (CMP) and some other additional capabilities. 

If the underlying data link layer supports multicasting, each sender sends a multicast neighbourhood solicitation to the **solicited-node multicasted address** of th target ( as opposed to a broadcast in ARP ). The target recipient will send back the sender with a neighbour advertisement. 












