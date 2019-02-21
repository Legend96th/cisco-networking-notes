  # Basics
  ---
  
  # What is a network
  It is a group of components connected together to provide a service
  ---
  
  ## Bus topology
  * Insecure 
  * Half duplex (only one device transmitting at a time)
  
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
  
  ## Unicast
  One to one
  
  ## Multicast
  One to many
  
  ## Broadcast
  One to all
  
  ---
  # MAC address
  
  * 48 bit
  * Represented in hexadecimal with '.' as a delimiter 
  * Composed of 6 fields of 2 hexadigits or 3 fields of 4 hexadigits
  * 7th bit is called U/L bit: set to 0 for a universally unique MAC address and 1 for a locally unique MAC address
  
  
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
  
  # OSI layers
  
  * Application (7)
    * [Data]
    * User interface
  * Presentation (6)
    * [Data]
    * ASCII
    * Voice: mp3
    * Video: mpeg
    * Compression
    * Encryption
  * Session (5) 
    * [Data]
    * Open path
    * Manage path
    * End path
  * Transport (4)
    * [Segment]
    * Adds a header to the data
      * Each message (segment) (header + data) is made of 1480 byte
      * If a message is larger than 1500 it get segmented
      * The header contains:
        * SYN: transmission method: 
          * Transmission Control Protocol(TCP)
          * User Datagram Protocol (UDP)
        * Sequence number: message/segment
        * Source & destination port      
        * CRC (2bytes): Cyclic Redundency 
          * Hashing result of all the segment
  * Network (3)
    * [Packet]
    * [Router]
    * Layer 3 || L3
    * Adds source & destination IP to the segment (20 byte)
    * Size of packet 1500 byte
  * Data-Link (2)
    * [Frame]
    * [Switch]
    * Layer 2 || L2
    * Adds source & destination Mac addresses (14 byte)
    * Adds another CRC (4 byte)
  * Physical (1)
    * [Bit]
    * [Physical support]
  
  **Note**: 
  * CPU works from Application (L7) => Network (L3)
  * NICs work in L2 & L1
  * PDU: Protocol Data Unit; Layer unit
  ---
  
  # TCP/IP layers
  
  * Application, Presentation & Session => Application
  * Transport => Transport
  * Network => Internet
  * Data-Link & physical => Link Access
  
  ---
  
  # TCP
  
  * Test the connexion before start sending data
  * Synchornisation (data sent) (Syn)
  * Acknoledgment (data well received) (Ack)
  
  Sender: Syn => 
  Receiver: <= Syn + Ack 
  Snder: Ack =>
  
  * Sliding window: the amount of first packets sent before receiver a Syn+Ack from the receiver
    * The first packets are sent before confirmation of the link
  ...
  
  ---
  
  # Switch
  
  * MAC address table starts emptey
  * When a device sends a message it gets registered
  * The message is broadcasted to all the devices
  * When a reply is sent, the responder is registered
    
    
    CAM (Content Addressable Memory) == MAC addresss table
  
  **Note**:
  * 2 types of switches
    * L2 switches: basic switch
    * Multi Layer Switch (MLS): can reach up to L4 & do some basic routing
  * Forwarding rates: number of ports * speed of port
  * POE: Power Over Ethernet
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
  
  ---
    
  # DHCP
  
  Work with the **DORA** principle
    
  * Discover 
    * Client send discovery packet
    * Broadcast
  * Offer
    * Server responds with a possible offer (an IP address that the client can use)
  * Request
    * Client requests that IP address
  * Acknoledgement
    * Server confirms the address
  
  Must setup the server's address before it can work
  
  * DHCP relay agent
    * A router that has been configured to forward DHCP discover messages to a specefic destination IP address or specefic destination network
  * If the DHCP server is on another subnet, the router's interface facing the network must be configured with `Router(config-if)#ip helper-address <ip_address>` 
  * DHCP option 82: Causes a DHCP Request packet to contain information indicating the switch port from which the DHCP request came from
    
  
  
  
  
  
  
'''
