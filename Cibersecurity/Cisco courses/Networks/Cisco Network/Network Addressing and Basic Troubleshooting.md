# Network Addressing and Basic Troubleshooting #
## 1 - Phisical Layer##

### Purpose of the Pysical layer ###
Before any network communications can occur, a physical connection to a local network must be established. A physical connection can be a wired connection using a cable or a wireless connection using radio waves. Network Interface Cards (NICs) connect a device to the network. Ethernet NICs are used for a wired connection, whereas WLAN (Wireless Local Area Network) NICs are used for wireless connections. The OSI physical layer provides the means to transport the bits that make up a data link layer frame across the network media. This layer accepts a complete frame from the data link layer and encodes it as a series of signals that are transmitted onto the local media. The encoded bits that comprise a frame are received by either an end device or an intermediary device.
### Phisical Layer Characteristics ###
The physical layer consists of electronic circuitry, media, and connectors developed by engineers. The physical layer standards address three functional areas: physical components, encoding, and signaling. Bandwidth is the capacity at which a medium can carry data. Digital bandwidth measures the amount of data that can flow from one place to another in a given amount of time. Throughput is the measure of the transfer of bits across the media over a given period of time and is usually lower than bandwidth. Latency refers to the amount of time, including delays, for data to travel from one given point to another. Goodput is the measure of usable data transferred over a given period of time. The physical layer produces the representation and groupings of bits for each type of media as follows:

- **Copper cable** - The signals are patterns of electrical pulses.
- **Fiber-optic cable** - The signals are patterns of light.
- **Wireless** - The signals are patterns of microwave transmissions.


### Copper Cabling ###
Networks use copper media because it is inexpensive, easy to install, and has low resistance to electrical current. However, copper media is limited by distance and signal interference. The timing and voltage values of the electrical pulses are also susceptible to interference from two sources: EMI and crosstalk. Three types of copper cabling are: UTP, STP, and coaxial cable (coax). UTP has an outer jacket to protect the copper wires from physical damage, twisted pairs to protect the signal from interference, and color-coded plastic insulation that electrically isolates wires from each other and identifies each pair. The STP cable uses four pairs of wires, each wrapped in a foil shield, which are then wrapped in an overall metallic braid or foil. Coaxial cable gets its name from the fact that there are two conductors that share the same axis. Coax is used to attach antennas to wireless devices. Cable internet providers use coax inside their customers’ premises.
### UTP Cabling ###
UTP cabling consists of four pairs of color-coded copper wires that have been twisted together and then encased in a flexible plastic sheath. UTP cable does not use shielding to counter the effects of EMI and RFI. Instead, cable designers have discovered other ways that they can limit the negative effect of crosstalk: cancellation and varying the number of twists per wire pair. UTP cabling conforms to the standards established jointly by the ANSI/TIA. The electrical characteristics of copper cabling are defined by the Institute of Electrical and Electronics Engineers (IEEE). UTP cable is usually terminated with an RJ-45 connector. The main cable types that are obtained by using specific wiring conventions are Ethernet Straight-through and Ethernet Crossover. Cisco has a proprietary UTP cable called a rollover that connects a workstation to a router console port.

### Fiber-Optic Cabling ###
Optical fiber cable transmits data over longer distances and at higher bandwidths than any other networking media. Fiber-optic cable can transmit signals with less attenuation than copper wire and is completely immune to EMI and RFI. Optical fiber is a flexible, but extremely thin, transparent strand of very pure glass, not much bigger than a human hair. Bits are encoded on the fiber as light impulses. Fiber-optic cabling is now being used in four types of industry: enterprise networks, FTTH, long-haul networks, and submarine cable networks. There are four types of fiber-optic connectors: ST, SC, LC, and duplex multimode LC. Fiber-optic patch cords include SC-SC multimode, LC-LC single-mode, ST-LC multimode, and SC-ST single-mode. In most enterprise environments, optical fiber is primarily used as backbone cabling for high-traffic point-to-point connections between data distribution facilities and for the interconnection of buildings in multi-building campuses.
## 2 - Data link layer ###

**Topologies**

The two types of topologies used in LAN and WAN networks are physical and logical. The data link layer "sees" the logical topology of a network when controlling data access to the media. The logical topology influences the type of network framing and media access control used. Three common types of physical WAN topologies are: point-to-point, hub and spoke, and mesh. Physical point-to-point topologies directly connect two end devices (nodes). Adding intermediate physical connections may not change the logical topology. In multiaccess LANs, nodes are interconnected using star or extended star topologies. In this type of topology, nodes are connected to a central intermediary device.

Physical LAN topologies include: star, extended star, bus, and ring. Half-duplex communications exchange data in one direction at a time. Full-duplex sends and receives data simultaneously. Two interconnected interfaces must use the same duplex mode or there will be a duplex mismatch creating inefficiency and latency on the link. Ethernet LANs and WLANs are examples of multiaccess networks. A multiaccess network is a network that can have multiple nodes accessing the network simultaneously.

Some multiaccess networks require rules to govern how devices share the physical media. There are two basic access control methods for shared media: contention-based access and controlled access. In contention-based multiaccess networks, all nodes are operating in half-duplex. There is a process if more than one device transmits at the same time. Examples of contention-based access methods include: CSMA/CD for bus-topology Ethernet LANs and CSMA/CA for WLANs.
## 3 - Routing at the network layer ##
**How a Host Routes**  
A host can send a packet to itself, another local host, and a remote host. In IPv4, the source device uses its own subnet mask along with its own IPv4 address and the destination IPv4 address to determine whether the destination host is on the same network. In IPv6, the local router advertises the local network address (prefix) to all devices on the network, to make this determination. The default gateway is the network device (i.e. router) that can route traffic to other networks. On a network, a default gateway is usually a router that has a local IP address in the same address range as other hosts on the local network, can accept data into the local network and forward data out of the local network, and route traffic to other networks. A host routing table will typically include a default gateway. In IPv4, the host receives the IPv4 address of the default gateway either dynamically via DHCP or it is configured manually. In IPv6, the router advertises the default gateway address, or the host can be configured manually. On a Windows host, the **route print** or **netstat -r** command can be used to display the host routing table.

**Routing Tables**  
When a host sends a packet to another host, it consults its routing table to determine where to send the packet. If the destination host is on a remote network, the packet is forwarded to the default gateway which is usually the local router. What happens when a packet arrives on a router interface? The router examines the packet’s destination IP address and searches its routing table to determine where to forward the packet. The routing table contains a list of all known network addresses (prefixes) and where to forward the packet. These entries are known as route entries or routes. The router will forward the packet using the best (longest) matching route entry.

The routing table of a router stores three types of route entries: directly connected networks, remote networks, and a default route. Routers learn about remote networks manually, or dynamically using a dynamic routing protocol. Static routes are route entries that are manually configured. Static routes include the remote network address and the IP address of the next hop router. OSPF and EIGRP are two dynamic routing protocols. The **show ip route** privileged EXEC mode command is used to view the IPv4 routing table on a Cisco IOS router. At the beginning of an IPv4 routing table is a code that is used to identify the type of route or how the route was learned. Common route sources (codes) include:

**L** - Directly connected local interface IP address  
**C** - Directly connected network  
**S** - Static route was manually configured by an administrator  
**O** - Open Shortest Path First (OSPF)  
**D** - Enhanced Interior Gateway Routing Protocol (EIGRP)

## 4 - IPv6 Addressing ##
### IPv6 Adress Types ###
There are three types of IPv6 addresses: unicast, multicast, and anycast. IPv6 does not use the dotted-decimal subnet mask notation. Like IPv4, the prefix length is represented in slash notation and is used to indicate the network portion of an IPv6 address. An IPv6 unicast address uniquely identifies an interface on an IPv6-enabled device. IPv6 addresses typically have two unicast addresses: GUA and LLA. IPv6 unique local addresses have the following uses: they are used for local addressing within a site or between a limited number of sites, they can be used for devices that will never need to access another network, and they are not globally routed or translated to a global IPv6 address. IPv6 global unicast addresses (GUAs) are globally unique and routable on the IPv6 internet. These addresses are equivalent to public IPv4 addresses. A GUA has three parts: a global routing prefix, a subnet ID, and an interface ID. An IPv6 link-local address (LLA) enables a device to communicate with other IPv6-enabled devices on the same link and only on that link (subnet). Devices can obtain an LLA either statically or dynamically.
### GUA and LLA configuration ###
The Cisco IOS command to configure an IPv4 address on an interface is ip address ip-address subnet-mask. In contrast, the command to configure an IPv6 GUA on an interface is ipv6 address ipv6-address/prefix-length. Just as with IPv4, configuring static addresses on clients does not scale to larger environments. For this reason, most network administrators in an IPv6 network will enable dynamic assignment of IPv6 addresses. Configuring the LLA manually lets you create an address that is recognizable and easier to remember. Typically, it is only necessary to create recognizable LLAs on routers. LLAs can be configured manually using the ipv6 address ipv6-link-local-address link-local command.
### Dynamic addressing for IPv6 GUAs ###
A device obtains a GUA dynamically through ICMPv6 messages. IPv6 routers periodically send out ICMPv6 RA messages, every 200 seconds, to all IPv6-enabled devices on the network. An RA message will also be sent in response to a host sending an ICMPv6 RS message, which is a request for an RA message. The ICMPv6 RA message includes: network prefix and prefix length, default gateway address, and the DNS addresses and domain name. RA messages have three methods: SLAAC, SLAAC with a stateless DHCPv6 server, and stateful DHCPv6 (no SLAAC). With SLAAC, the client device uses the information in the RA message to create its own GUA because the message contains the prefix and the interface ID. With SLAAC with stateless DHCPv6 the RA message suggests devices use SLAAC to create their own IPv6 GUA, use the router LLA as the default gateway address, and use a stateless DHCPv6 server to obtain other necessary information.

With stateful DHCPv6 the RA suggests that devices use the router LLA as the default gateway address, and the stateful DHCPv6 server to obtain a GUA, a DNS server address, domain name and all other necessary information. The interface ID can be created using the EUI-64 process or a randomly generated 64-bit number. The EUIs process uses the 48-bit Ethernet MAC address of the client and inserts another 16 bits in the middle of MAC address to create a 64-bit interface ID. Depending upon the operating system, a device may use a randomly generated interface ID.
### Dynamic addressing for IPv6 LLAs ###
All IPv6 devices must have an IPv6 LLA. An LLA can be configured manually or created dynamically. Operating systems, such as Windows, will typically use the same method for both a SLAAC-created GUA and a dynamically assigned LLA. Cisco routers automatically create an IPv6 LLA whenever a GUA is assigned to the interface. By default, Cisco IOS routers use EUI-64 to generate the Interface ID for all LLAs on IPv6 interfaces. For serial interfaces, the router will use the MAC address of an Ethernet interface. To make it easier to recognize and remember these addresses on routers, it is common to statically configure IPv6 LLAs on routers. To verify IPv6 address configuration use the following three commands: show ipv6 interface brief, show ipv6 route, and ping.
### IPv6 Multicast address ###
There are two types of IPv6 multicast addresses: well-known multicast addresses and solicited-node multicast addresses. Assigned multicast addresses are reserved multicast addresses for predefined groups of devices. Well-known multicast addresses are assigned. Two commonIPv6 assigned multicast groups are: ff02::1 All-nodes multicast group and ff02::2 All-routers multicast group. A solicited-node multicast address is similar to the all-nodes multicast address. The advantage of a solicited-node multicast address is that it is mapped to a special Ethernet multicast address.


## 5 - IPv6 Neighbor Discovery ##
**Neighbor Discovery Operation**  
IPv6 does not use ARP, it uses the ND protocol to resolve MAC addresses. ND provides address resolution, router discovery, and redirection services for IPv6 using ICMPv6. ICMPv6 ND uses five ICMPv6 messages to perform these services: neighbor solicitation, neighbor advertisement, router solicitation, router advertisement, and redirect. Much like ARP for IPv4, IPv6 devices use IPv6 ND to resolve the MAC address of a device to a known IPv6 address.

## 6- Cisco switches and routers ##
### Cisco switches ###
A switch is used to connect devices on the same network. A router is used to connect multiple networks to each other. When selecting a switch for your LAN, choosing the appropriate number and type of ports is critical. Lower-cost switches may support only copper twisted-pair interface ports. Higher priced switches may have fiber-optic connections. These are used to link the switch to other switches that may be located over long distances.

Similar to a switch port, Ethernet NICs operate at specific bandwidths such as 10/100 or 10/100/1000 Mbps. The actual bandwidth of the attached device will be the highest common bandwidth between the device NIC and the switch port. Networking devices come in both fixed and modular physical configurations. A managed switch that uses a Cisco operating system enables control over individual ports or over the switch as a whole. Cisco Catalyst 2960 Series Ethernet switches are suitable for small and medium-sized networks.
### Switch speeds and forwarding methods ###
Switches use one of the following forwarding methods for switching data between network ports: store-and-forward switching or cut-through switching. Two variants of cut-through switching are fast-forward and fragment-free. Two methods of memory buffering are port-based memory and shared memory. There are two types of duplex settings used for communications on an Ethernet network: full-duplex and half-duplex.

Autonegotiation is an optional function found on most Ethernet switches and NICs. It enables two devices to automatically negotiate the best speed and duplex capabilities. Full-duplex is chosen if both devices have the capability along with their highest common bandwidth. Most switch devices now support the automatic medium-dependent interface crossover (auto-MDIX) feature. When enabled, the switch automatically detects the type of cable attached to the port and configures the interfaces accordingly.
### Switch boot process ###
Cisco switches are preconfigured to operate in a LAN as soon as they are powered on. Configure the basic security settings before placing the switch into the network. The three basic steps for powering up a switch are as follows: 1) Check the components, 2) Connect the cables to the switch, and 3) Power up the switch. When the switch is on, the power-on self-test (POST) begins.

There are two methods to connect a PC to a network device to perform configuration and monitoring tasks: out-of-band management and in-band management. Out-of-band management requires a computer to be directly connected to the console port of the network device that is being configured. Use in-band management to monitor and make configuration changes to a network device over a network connection.

A Cisco device loads the following two files into RAM when it is booted: the IOS image file and the startup configuration file. The IOS image file is stored in flash memory. The startup configuration file is stored in NVRAM.
### Cisco routers ###
Routers require an OS, a CPU, RAM, ROM, and NVRAM. Every Cisco router has the same general hardware components: console ports, LAN interfaces, expansion slots for different types of interface modules (e.g., EHWIC, Serial, DSL, switch ports, wireless), storage slots for expanded capabilities (e.g., compact flash memory, USB ports).
### Router boot process ###
Follow these steps to power up a Cisco router:

- **Step 1**. Securely mount the device to the rack.
- **Step 2**. Ground the device.
- **Step 3**. Connect the power cable.
- **Step 4**. Connect a console cable.
- **Step 5**. Turn on the router.
- **Step 6**. Observe the startup messages on the PC within the terminal window as the router boots.

The most common methods to access the command line interface on a Cisco router are console, SSH, and Aux ports. Routers also have network interfaces to receive and forward IP packets.
### 7 Troubleshoot common network problems ###
#### Network types ####

The internet is not owned by any individual or group. The internet is a worldwide collection of interconnected networks (internetwork or internet for short), cooperating with each other to exchange information using common standards. Through telephone wires, fiber-optic cables, wireless transmissions, and satellite links, internet users can exchange information in a variety of forms.

Small home networks connect a few computers to each other and to the internet. The SOHO network allows computers in a home office or a remote office to connect to a corporate network, or access centralized, shared resources. Medium to large networks, such as those used by corporations and schools, can have many locations with hundreds or thousands of interconnected hosts. The internet is a network of networks that connects hundreds of millions of computers world-wide.

There are devices all around that you may interact with on a daily basis that are also connected to the internet. These include mobile devices such as smartphones, tablets, smartwatches, and smart glasses. Things in your home can be connected to the internet such as a security system, appliances, your smart TV, and your gaming console. Outside your home there are smart cars, RFID tags, sensors and actuators, and even medical devices which can be connected.
#### Data transmission ####

The following categories are used to classify types of personal data:

- **Volunteered data** - This is created and explicitly shared by individuals, such as social network profiles. This type of data might include video files, pictures, text, or audio files.
- **Observed data** - This is captured by recording the actions of individuals, such as location data when using cell phones.
- **Inferred data** - This is data such as a credit score, which is based on analysis of volunteered or observed data.

The term bit is an abbreviation of “binary digit” and represents the smallest piece of data. Each bit can only have one of two possible values, 0 or 1.

There are three common methods of signal transmission used in networks:

- **Electrical signals** - Transmission is achieved by representing data as electrical pulses on copper wire.
- **Optical signals** - Transmission is achieved by converting the electrical signals into light pulses.
- **Wireless signals** - Transmission is achieved by using infrared, microwave, or radio waves through the air.
#### Bandwidth and troughput ####
Bandwidth is the capacity of a medium to carry data. Digital bandwidth measures the amount of data that can flow from one place to another in a given amount of time. Bandwidth is typically measured in the number of bits that (theoretically) can be sent across the media in a second. Common bandwidth measurements are as follows:

- Thousands of bits per second (Kbps)
- Millions of bits per second (Mbps)
- Billions of bits per second (Gbps)

Throughput does not usually match the specified bandwidth. Many factors influence throughput including:

- The amount of data being sent and received over the connection
- The types of data being transmitted
- The latency created by the number of network devices encountered between source and destination

Latency refers to the amount of time, including delays, for data to travel from one given point to another.

