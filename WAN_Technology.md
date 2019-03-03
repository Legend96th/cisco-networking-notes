  # WAN Technology
  
  # WAN protocols
  * Defines format of data to be sent on the WAN links
  * Also reffered as encapsulation
  * Technologies:
    * Circuit switching
      * HDLC
      * PPP
    * Packet switching
      * X.25
      * Frame relay
    * Cell switching
      * ATM
    * Frame switching
      * Ethernet
        * Metro Ethernet (MAN not WAN)
    * Label switching
      * MPLS
  
  * Switching protocols
    * ##:
      * Signal
      * Data
    * Types: 
      * OSPF, EIGRP, HDLC, PPP
  * Switched protocols 
    * IPv4, IPv6, Appletalk, IPx, MPLS
  
  * DTE: Data Terminal Equipment 
    * Receives clocking (speed limit)
    * ex: 
      * In Leased line setup: Router
      * In dial up setup: Computer
  * DCE: Data Communication Equipment 
    * Has a clock (generates a clock)
    * ISP side
    * Sets the rate (speed) of the net
    * 1 sec is divided to 8 TC / TS
      * Can be closed/opened to set the speed of the net
  * Local loop: The link between the ISP & the client
  * Demarcation point: Determines the responsibility zones 
    * if the ISP should fix it if something happens (external wires) or if you have to fix it urself (internal wires)
  * CPE: Customer Premessis Equipment
    * Devices of client
  
  
  ---
  # WAN topologies
  
  * Point-to-Point networks:
    * Direct connection between 2 router
    * Interconnects two devices
    * Typically uses a L2 protocol
    * Could be a physical or logical point-to-point connection
  * Hub and spoke
    * Cost effective for multiple sites
    * Could have suboptimal paths
    * Hub could be a single point of failure
  * Full mesh networks
    * Doesn't scale well
    * Optimal pathing
    * Higher expense
  
  ---
  # Signle Homed vs Dual Homed
  
  * Signle home: One link to one ISP
  * Dual home: Two or more links to one ISP
  * Single Multihomed: One link per ISP to two or more ISPs
  * Dual Multihomed: Two or more links to two or more ISPs
  
  ---
  
  # Communicating 2 networks passing by multiple routers connected with RJ45 cables
  
  * When the sender tries to sends a request to the receiver, it needs to have it's MAC @ so it sends an ARP request
  * Since it's on another network, the destination MAC @ is filled with the default gateway's MAC at this point
  * After it enters the router, the original packet is encapsulated as a payload & source & destination MAC @ of the new packet are replaced with the routers's MAC addresses (dest & source)
  * After it reaches the LAN of the destionation, the ARP request is broadcasted
  
  **If the routers are connected with Serial, it's not the same**
  
  ---
  
  # 1.Dedicated circuit switching (Leased line)
  Site to site private connexion
  
  * Digital Speed (DS)
    * DS0: 64Kbps
    * DS1: 
      * T1 (USA): 1.54 Mbps
      * E1 (Europe): 2.048 Mbps
    * DS2
    * DS3: 
      * T3 (USA): 45Mbps
      * E3 (Europe): 34Mbps
    * STM1: 155Mbps
    * STM64: 10Gbps 
    * STM256: 40Gbps
  STM is used by ISP to connect to the international backbone
  
  ---
  
  # 2.On demand circuit switching (Asynchronous serial)
  
  * Dial-up: (the old school method) use either phone or data (internet) at one time only
    * 64Kbps
  * ISDN: Can use voice (phone) and data (internet) at the same time or use voice subscription to send data (via the audio channel) & get double speed (64Kbps + 64Kbps)
    * 64Kbps/line (audio or data)
  
  ---
  
  # 3.Packet switching (Synchronous serial)
  
  * X.25
    * Reliable (detects errors)
    * Slow: 40Kbps
  Error detection with slow connection can be a big problem (slowing traffic more) 
  * Frame-relay
    * Supports serial cable only
    * Frame-relay topologies
      * Hub & spoke
        * All branches routers have to pass the FRSW (Frame-Relay switch), go to the HQ router to communicate with others
      * Partial Mesh
        * Connect 2 branches directly without passing by the FRSW & HQ
      * Full Mesh
        * Connect all branches directly 
  ---
  
  # 4.Cell switching
  
  * ATM (similar to Frame-Relay) 
    * Supports serial cable only
    * Speed: 40Gbps
    * Problems:
      * Very expensive
      * Limited by short distance 
  
  ---
  
  # 5.MPLS
  Multi protocol Lable Switching
  
  *check MPLS page*
  
  ---
  
  # 6.Broadband
  
  * DSL (Digital Subscriber Line)
    * ADSL: 
      * Users (clients) use download more than upload
      * The line is split to 3 unequal parts (some upload & some voice & the big remaining is download)
    * SDSL: 
      * Same speed for download & upload
  * Cable TV:
    * Using FTTH
    * IPTV
  * Satelite 
  
  ---
  
  # Modern WAN connections
  
  * MPLS
  * Metro ethernet
  * Virtual private network (VPN)
  * DSL
  * Cable
  * VSAT
  
'''
