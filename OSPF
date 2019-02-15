createdAt: "2018-09-11T09:01:00.708Z"
updatedAt: "2019-02-15T16:07:10.247Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "OSPF"
content: '''
  # OSPF
  Open Shortest Path First
  
  * Symbol: "O"
  * Link-State
    * Dynamic routing
  * VLSM
    * Classless (supports subnetting)
    * VLSM supports subnetting of subnets 
  * Summirization
    * Only works if all the subnets are consecutif 
    * eg: if we have a router attached to these networks: 10.0.0.0, 10.0.1.0, 10.0.2.0, 10.0.3.0, starting from the left, count the number of bits that are the same accross all the networks, that number is considered as a CIDR number, convert it to wildcard mask & use it only once with OSPF
    * 
  * AD = 110
    * Administrative distance
  * Metric (cost) = (Reference bandwidth) / (Interface bandwidth) 
    * Default reference bandwidth = 10^8 (100Mbps)
  * Establishes adjacencies & neighborships with other routers
  * Works with areas
  * Hello time: 10s
    * A message sent between routers to exchange info
  * Hello & updates are sent with the multicast address 224.0.0.5-6
    * LSA (Link State Advertisment) 
  * Hold down time: 40s (4 tries)
  * Sends Link State advertisements (LSAs) to other routers in an area
  * Constructs a link state database (LSDB) from received LSAs
  * All the routers in the same area should have the same LSDB
  * If the routes costs are the same use load balancing
  * Uses the loopback to see if the local OSPF is running correctly
  * Runs the Dijkstra Shortest Path First (SPF) algorithm to determine the shortest path to a network using link bandwidth as cost
  * Attempts to inject the best path for each network into a router's IP routing table
  * Neighbors:
    * Reside on the same network link
    * Exchange Hello messages (224.0.0.5 || FF02::5)
  * Adjacencies:
    * Are neighbors
    * Have exchanged Database Description (DD) packets and Link State Updates (LSUs)
    * Forming an Adjacency:
      * One way communication (sender or receiver at a given time)
        * Both (or one) routers are in the down state if there is no connectivity between
        * (both links are up) both router change to an "init state" R1 sends a Hello message to R2, when R2 receives it, it notices that it's router ID is not listed there. R2 adds R1 in it's neighbors table and then sends back a Hello message to R1
        * R1 receives a Hello message from R2 containing R1's router ID (R1 now knows that R2 has it in it's neighbors table). R1 adds R2 to it's neighbors table then, it switches to a 2-Way state. After that, it sends a Hello message back to R2
        * R2 receives a Hello message from R1 containing R2's router ID (R2 now confirms that R1 has it in it's neighbors table). R2 now is in a 2-Way state.
      * Two way communication (both send & receive at the same time)
        * State = **2-Way**. DR/BDR election is done (if needed)
        * State = **ExStart**. Primary / Secondary routers are selected
          * Primary: Sends the DD packet to the secondary router
          * Secondary: Sends an Ack to the primary router
        * State = **Exchange**. DD packets are exchanged between the routers
        * State = **Loading**. Routers query one another (using LSRs) for missing entries (sent in LSUs)
        * State = **Full**. Adjacency fully formed
  
  
  # OSPF packets
  
  * DD (DB description): List of LSAs known to a router
  * LSU (LS update)
    * LSA (LS Advertisment ) [**not a packet type** just a piece of information]
  * LSR (LS request)
  
  ***Note***
  *DB: Database
  LS: Link State*
  
  # OSPF Topology
  
  * Point to Point
    * Router to router
  * BMA (Broadcast Multi Access)
    * Router are inter-connected with a switch
    * In each group of router connected with a switch, a master router (called "Designated Router" - DR) & a backup master router (called "Backup DR" - BDR). A DR & BDR are selected based on the highest priority that values from 0 to 255, if the priorities are all the same, the highest IP address is elected as DR.
    * When a new network is connected, a message to the DR is sent via 224.0.0.6 (others dont listen to  this multicast). After that, the other routers are informed by the DR via 224.0.0.5.
    * The DR is responsible of receiving informations about new networks & sending them back to the other routers
    * The BDR is a backup to be used if the DR is down.
  
  ---
  
  # OSPF router types
  * **ABR** (Area Border Roter): A router with at least one interface in the backbone area and at least one interface in a non-backbone area
  * **ASBR** (Autonomous System Boundry Router): A router that connects to another routing protocol
  
  ---
  # Areas 
  
  * Multiple areas
  * We always have Area 0 which is called the backbone
  * A router that connects 2 or more areas is called ABR (Area Boundry Router)
  * ((Each area holds a maximum of 50 router)) 
  * Areas must be connected to area 0 to connect to all the other areas, otherwise that area will only connected to the directly connected areas
  * **Virtual-Link** can be used to connect to all areas without connecting to the area 0
  * An ABR has to apply the SPF algorithm for all the areas that it's linked to while an AR has to apply it for it's own area only 
  
  ---
  
  # Configuration
  
  ## OSPFv2
  
  
  * Configure an ospf router process (local & dosn't matter to other routers)
  ```
  Router(config)#router ospf <process-id>
  ```
  
  * Add a **single** participating interface (network or interface ip address)
  ```
  Router(config-router)#network <ip_address> <wildcard_mask> area <area-num>
  ```
  Or
  ```
  Router(config-if)#ip ospf <process-id> area <area-num>
  ```
  
  * Add **all** interfaces of a router to the same area
  ```
  Router(config-router)#network 0.0.0.0 255.255.255.255 area <area-num>
  ```
  
  * Show the participating interfaces
  ```
  Router#show ip ospf interface [brief]
  ```
  
  * Other usefull show commands 
  ```
  Router#show ip protocols
  ```
  ```
  Router#show ip ospf database
  ```
'''
tags: [
  "CISCO"
]
isStarred: false
isTrashed: false
linesHighlighted: []
