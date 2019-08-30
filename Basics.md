  # Basics
  ---
  
  # What is a network
  It is a group of components connected together to provide a service
  
  ---
  
  # Interface caracterstics
  
  * Duplex
    * Half duplex: Only one end transmitting at a time
    * Full duplex: Both ends send and receive at the same time
  * Speed
    * Number of bits that can go on the wire in a second
  
  ---
  # Topology types
  
  ## Bus topology
  * Insecure 
  * Half duplex 
  
  ## Ring topology
  * Eeach device requires 2 NICs
  * Uses token to define who has the ability to send data
  
  ## Full mush
  * Each device requires multiple NICs
  * Devices are interconnected between them
  
  ## Star topology
  * Uses only one Hub to communicate
  
  ## Extended star topology
  * Uses multiple Hubs
  ---
  
  # Network architecture
  
  
  ## Three-Tier
  * Divided into three layers:
    * Access
      * L2 ethernet switches
      * Star topology
    * Distribution
      * L3 switch
      * Partial mesh topology
    * Core
      * Moving packets asap between the distribution layer switches
      * Exiting to the internet
      * Partial mesh topology
  
  ## Collapsed core
  Two-Tier topology
  * Same as Three-Tier if we combine the distribution & core layers
  
  
  ---
  
  # Network types
  
  ## LAN (Local Area Network)
  Same building
  
  ## MAN (Metropolitan Area Network)
  Same country
  
  ## WAN (Wide Area Network)
  Different countries
  ---
  
  # Communication types
  
  * Unicast: One to one
  * Multicast: One to many 
  * Broadcast: One to all 
  * Flooding: One to all, except sender
  
  ---
  # MAC address
  
  * 48 bit
  * Represented in hexadecimal with '.' as a delimiter 
  * Composed of 6 fields of 2 hexadigits or 3 fields of 4 hexadigits
  * 7th bit is called U/L bit: set to 0 for a universally unique MAC address and 1 for a locally unique MAC address
  
  
  --- 
  
  # IPv4
  Internet Protocol v4
  
  * Resides at the OSI L3
  * Routed protocol
  * Connectionless (doesn't verifiy if the host is up or if it received the data)
  * 32-bit addressing system (source and destination IP address)
  * Logical addressing
  * Dotted decimal representation (0-255.0-255.0-255.0-255)
  * Header components:
    * Version
    * IHL (IP Header Length): Number of sections of 32 bits in the header
    * ToS (Type Of Service): 
      * QoS related
      * Composition
        * DSCP
        * ECN
    * L3 fragmentation fields:
      * Composition:
        * Total length: IHL + data length
        * Identification: ID of the L3 packet
        * Flags (3 bits)
          * Bit 0: Reserved
          * Bit 1: DF (Don't Fragment): If a packet needs to be fragmented while DF=1 it'll be dropped
          * Bit 2: MF (More Fragments):
            * MF=1: Expect more fragments to come
            * MF=0: Last fragment
        * Fragment offset: Position of the L3 fragment
      * These fields are used if L3 fragmentation is used
        * If a packet meets a lower MTU on it's way to the destination it'll be fragmented
    * TTL (Time To Live): 
      * A counter that is decremented on every router to avoid L3 routing loops
      * When a packet is received TTL is decremented by 1. If TTL=0 the packet is dropped
    * Protocol number: The L4 receiving protocol ID
      * Examples:
        * ICMP = 1
        * IGMP = 2
        * TCP = 6
        * UDP = 17
        * EIGRP = 88
        * OSPF = 89
    * Header checksum: Used for error detection
    * Source IP address
    * Destination IP address
    * Options:
      * A field that is not often used
      * Variable length
      * If IHL > 5 then options are used
      * Copied: if = 1 => copy options on all fragments
      * Option types:
        * control: 0
        * reserver: 1
        * debugging and measurement: 2
        * reserved: 3
      * Option number
      * Option length (optional for some options)
      * Option data (optional for some options)
    * Padding: If the EOL (End Of Options List) doesn't meet the data start it gets filled with 0s
  
  
  
  
  
  ---
  
  # Types of IPv4 addresses
  
  * Public address
    * Used on public network (interner)
    * Recognized on internet
    * Given by ISP from IANA
    * Globally unique
    * Not free
    * Registered
  * Private address
    * Used in LANs
    * Not recognized on the internet
      * Not routable by the ISP
    * Given by the network administrator (or DHCP)
    * Unique within the network
    * Free
    * Unregistered
    * Ranges:
      * Class A: 10.0.0.0    to 10.255.255.255
      * Class B: 172.16.0.0  to 172.31.255.255
      * Class C: 192.168.0.0 to 192.168.255.255
  
  **Note:**
  In class B, addresses from 169.254.0.0 to 169.254.255.255 APIPA (Automatic Private IP Addressing) are **NOT** routable even in a LAN!
  
  ---
  
  # Reserved addresses
  
  * 0.x.x.x
  * Current network: 0.0.0.0
  * Loopback: 127.x.x.x 
  * APIPA: 169.254.0.0 to 169.254.255.255
  
  
  ---
  # Ethernet LANs fundamentals
  
  Ethernet refers to a family of IEEE standards that define both physical and Data-Lik standards (cabeling, connectors, protocols and rules for the **wired** LANs)
  Ethernet only supports wired LANs
  
  
  ---
  
  # Medias types (types of cables)
  * Fiber
    * Monomode
    * Multimode
  * Copper
    * Coaxial
      * Thin (500m)
      * Thick (200m)
    * Twisted pair
      * Shielded
      * Unshielded
  ---
  # Protocols 
  
  In general protocols are:
  * A set of rules of operation
  * Facilitate vendors interoperability
  * Easier troubleshooting (by knowing where a problem occured, we can rule out some fake causes)
  
  
  ---
  
  # OSI layers
  
  * Application (7)
    * [Data]
    * Network process to applications
    * Provides an interface between the end-user programs (applications) and the network
  * Presentation (6)
    * [Data]
    * Data representation and encryption
    * Defines how data is presented (ASCII), stored and sent (clear/encrypted)
    * Unifies the communication between different vendors
    * Coding
    * Extension
      * Voice: mp3
      * Video: mpeg
    * Compression
    * Encryption
  * Session (5) 
    * [Data]
    * Inter-host communication
    * Keeps track of sessions opened between 2 hosts
    * Uses ports to map sessions to applications (
      * Random source (mostly) 
      * Defined destination
    * Open path
    * Manage path
    * End path
  * Transport (4)
    * [Segment]
    * End-to-End connection and reliability
    * Segmentation ( Fagmentation )
      * The **pure** data is segmented starting from (46 to 1460) Byte
    * Adds a header to the data
      * Each message (segment) (header + data) is made of 1480 byte
      * If a message is larger than the MTU (Maximum Transfer Unit) (1500 by default) it gets segmented
    * Segment contains:
        * [DATA]
        * Source & destination port      
        * SYN: transmission method: 
          * Transmission Control Protocol(TCP) 20 Byte
          * User Datagram Protocol (UDP) 8 Byte
        * Addressing Sequence number: message/segment
        * CRC (2bytes): Cyclic Redundency 
          * Hashing result of all the segment
    * Size of segment: 1480 Byte
  * Network (3)
    * [Packet]
    * [Router]
    * Layer 3 || L3
    * Path determination and logical addressing
    * Segmentation of network topology into logical partitions (make network groups that only see the same group traffic)
    * Logical addressing (doesn't follow the devices, changers by user)
    * Path discovery and selection
      * Responsible for knowing the 
        * Existing network partitions
        * Optimal paths to take to move from one partition to another
  * Data-Link (2)
    * [Frame]
    * [Switch]
    * Layer 2 || L2
    * MAC and LLC (physical addressing)
    * Media-Access control: when and how to put bits on the wire (collision detection...etc)
    * Link-Layer (L2) addressing: used on communication on the same link between 2 or more hosts 
    * Error checking: check if the data wasn't altered (cable damage, electrical interferrance...etc)
    * Example:
      * Ethernet protocol
    * Frame contains
      * Packet
      * Source & destination Mac addresses (14 byte)
      * Adds another CRC (4 byte)
      * Preamble (8 Bits): Sent on the first frame only to inform that frames will be coming and to test the speed
  * Physical (1)
    * [Bit]
    * [Physical support]
    * Media, signal and binary transmission
    * (Electrical, light, electromagnetic...etc) signals carried over the physical layer
  
  **Note**: 
  * CPU works from Application (L7) => Network (L3)
  * NICs work in L2 & L1
  
  ---
  
  # TCP/IP layers
  
  * Application (Application, Presentation & Session):
    * Provides services to the applications used by users. 
    * Doesn't define the applications themselves.
    * Examples:
      * DHCP
      * DNS
      * HTTP
      * HTTPs
      * SMTP
  * Transport (Transport)
    * Provides a set of protocols that define how data is transported in the network
    * Examples:
      * TCP
      * UDP
      * OSPF
      * EIGRP
  * Internet (Network)
    * Groups a small number of protocols out of which is the famous IP protocol
  * Link Access (Data-Link & physical)
    * Dfines the protocols and hardware required to deliver data accross some physical network
  
  ---
  # Layers interaction
  
  * Same layer interaction:
    * Happens between 2 devices 
    * Communicate, verify and set the rules of transmission on that layer
  * Different layer interaction:
    * Happens on the same device
    * A higher layer requsts  a service or a functionality from a lower layer
  
  ---
  # PDU
  Potocol Data Unit
  
  
  * The final structured data unit created by an OSI layer
  * PDUs created at one layer are meant to be read by the same layer on the receiving device
  * Layer PDU naming:
    * L4:
      * TCP: segment
      * UDP: datagram
    * L3: Packet
    * L2: 
      * Frame
      * Cell (ATM)
  * Also noted as Li PDU ; i = 1..7
  
  
  ---
  
  # Encapsulation / decapsulation
  
  * Encapsulation: Adding headers to PDUs received from the upper layer
  * Decapsulation: Removing headers from PDUs received from the lower layer
  
  | TCP/IP Layer | PDU name        | Structure                                         | Groupped structure               |
  | ------------ | --------------- | ------------------------------------------------- | -------------------------------- |
  | Application  | User data       | DATA                                              | DATA                             |
  | Transport    | Segment         | Transport \\| DATA                                 | Transport \\| DATA                |
  | Network      | Packet          | IP \\| Transport \\| DATA                           | IP \\| Segment                    |
  | Data-link    | Frame           | Data-Link \\| IP \\| Transport \\| DATA \\| Data-Link | Data-Link \\| Packet \\| Data-Link |
  | Physical     | *transmit bits* | *transmit bits*                                   | *transmit bits*                  |
  
  
  ---
  
  # TCP
  Transmission Control Protocol
  
  * **Protocol number 6**
  * Caracteristics
    * Sequencing and reassembling
    * Windowing, buffering, congestion avoidance 
    * Connection oriented
      * Check if the receiver is up before sending data
    * **Reliable**
      * Check if the receiver received the data
    * Check how much data is being dropped
    * Error correction
  * Control message types
    * Synchornisation (data sent) (Syn)
    * Acknoledgment (data well received) (Ack)
  * Sliding window: the amount of first packets sent before receiving a Syn+Ack from the receiver
    * The first packets are sent before confirmation of the link
  * Steps (sessions):
    * Session establishment 
      * Uses 3-Way handshake
        * Sender: Syn => 
        * Receiver: <= Syn + Ack + Window
        * Snder: Ack =>
    * Session control and management:
      * Based on the window, the sender sends the segments
      * The receiver sends an ACK with the segment that it needs with a window specifying how many segments to send (ACK + window)
        * If the frames are received correctly the receiver sends an ACK with the next segment and a window size
        * If a frame isn't received, the receiver sends an ACK with the missing frame with a window of 1
    * Session termination
      * 4-Way handshake
        * => FIN
        * <= ack
        * <= fin
        * => ack
  * Header components:
    * TCP source port
    * TCP destination port
    * Sequence number
    * Acknowledgment number
    * Header length (data offset)
    * Reserved
    * Control flags:
      * NS: 
  ---
  # UDP
  User Datagram Protocol
  
  * **Protocol number 17**
  * Connectionless
  * Unreliable
  * Header components:
    * UDP source port number
    * UDP destination port number
    * Length (header + data)
    * Checksum (UDP checksum)
  
  
  ---
  
  # Protocol number vs Port number
  
  * Protocol number:
    * Layer 3
    * Specifies which protocol we need to communicate with (TCP, UDP, OSPF, EIGRP...etc); the protocol that should receive the payload
  * Port number:
    * Layer 4
    * Specifies which application should receive the payload
  
  ---
  
  # Routed protocols vs Routing protocols
  
  * Routed protocols:
    * Protocols that are used for **identification**
    * Examples:
      * IP
      * IPX
      * AppleTalk
  * Routing protocols
    * Protocols that determine the best paths for the routed protocls
    * Divided in 2
      * Network identification
      * Host identification
    * Examples:
      * RIP
      * OSPF
      * EIGRP
  
  
  
  
  ---
  
  # Device memory types
  
  * Flash: IOS
    * Non volatile
    * Slow
    * Size: moderate
  * NVRAM: Startup config
    * Non volatile
    * Slow
    * Size: small
  * DRAM: Running config
    * Volatile
    * Fast
    * Size: large
  
  ---
  
  # Network Devices
  
  * Switch
  * Router
  * IPS: Intrusion Prevntion System
    * Prevents attacks by dropping suspecious incoming packets from entering the network
    * Sits in line with the traffic
    * It's the first device after the router connected to an unsecure connection
    * Uses a large database of well known attack header signatures
  * IDS: Intrusion Detection System
    * Receives a copy of the traffic on the network
    * Can recognize an attack using a pattern
    * Can stop an attack by sending an instruction to the router to stop receiving packets from the source of the attack
    * Sits with the end devices (as an endpoint)
    * Uses a large database of well known attack header signatures
  * Firewalls
    * Types:
      * Packet filter: Permit or deny traffic based on source/destination IP & port numbers
      * Stateful firewall: In addition to packet filtering, it can inspect sessions and recognize return traffic for a reason that was initiated from a trusted network
      * Application layer firewall: In addition to stateful, it understands the nature of an application (if it uses different protocols)
  * Wireless Access points
    * Types:
      * Autonomous AP: An AP that is managed individually
      * Lightweight AP: An AP that can be managed in group with a wireless LAN controller
  
  ---
  
  # CISCO IOS 
  Internetworking Operating System
  
  
  * Images are divided into
    * Hardware platform
    * Features
    * Versions
  * Naming conventions
    * Versions:
      * Train: Major version 
      * Throttle: Minor version
      * Rebuilds: Bug fixes
    * Revisions:
      * Mainline (M): Most stable version
      * Technology Train (T): New features, more bugs
  
  ---
  
  # Subnetting
  
  ```
  1. Magic number:
    * 256 - <mask> = <start_of_2nd_network>
    * size of range = <start_of_2nd_network> - 1
  2. Cool method:
    * Convert mask to binary
    * Last 1 to the right = <start_of_2nd_network>
  
  Example: 
    255.255.255.128
  1. Magic number 
    * 256 - 128 = 128
    * Net1: 0 -> 127
    * Net2: 128 -> 255 
  2. Cool method
    * 255.255.255.128 = 1-1.1-1.1-1.10000000
    * 10000000
    * ^ 
    * 128
    * Net1: 0 -> 127
    * Net2: 128 -> 255 
  ```
    
  
  
  
  
  
  
'''
