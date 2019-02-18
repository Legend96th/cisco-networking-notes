  
  # Routing
  
  * Routing sources
    * Connected
    * Static routing
    * Dynamic routing
    * Default route
  
  ##### Notes
  * Routers stop broadcasts from crossing networks
  
  ---
  
  # Static routing
  
  Notes:
  * Static routes won't show up on the routing table if the next hop IP address is unavailable or the egress interface is down. To make a static route appear on the routing table even if that happend, add the keyword `permanent` at the end of the route addition command
  * If using IPv6, a next hop Link-Local IPv6 address cannot be used on it's own, the egress interface must be specified aswell 
  
  ---
  
  # Dynamic routing
  
  ## IGP Interior Gateway Protocol
    * Distance Vector: This method choose the route with the least amount of hops. If 2 routes have the same amount of hops, load balancing is used || It sends the whole routing table to it's directly connected neighbors periodically even if there has been no network changes || It doesnt have an overview of the whole network
      * RIP (Routing Information Protocol)
        * IEEE 
        * Metric: hop counts
        * Exchange the **entire** routing table (address + number of hops) every 30 secs or when a change in the topology occurs (full & triggered updates)
        * The max hops that can be done by RIP is 15 hops (16 router passed)
        * The path with the shortest hop count maybe he slowest
        * Load balancing up to 4 equal paths
        * Uses the poison reverse rule: If a router knows that one of his directly connected routes is unavailiable, it sends an update with a metric of 16 to inform other routers that it no longer exists
        * Uses the split horizon rule: If a route is received from a router, it is not sent back to it with the routing table
        * Uses route summerization by default (can lead to some errors). 
          * The summerization is class based (the route summary will be a classful route not classless)
        * RIPv1:
          * Classful routing only (**NO subnetting**)
          * Sends updates in broadcast via 255.255.255.255
          * Timers:
            * Update timer: 30sec
            * Hold down timer: 180sec [The directly connected link becomes unavailable]
            * Invalid timer: **180sec** (30+150) [The routes received from a router that hasn't sent updates in 180sec are **considered unusable**]
            * Flush timer: **240sec** (180+60) [The routes received from a router that hasn't sent updates in 240sec are **removed**]
        * RIPv2
          * Uses multicast 224.0.0.9
          * Only RIPv2 received update
          * 16 hops only
          * Supports subnetting
            * If using subnetting, auto summary must be disabled to avoid having errors (if multiple routes can be summarized but they are not exiting the same interface, it'll genrate an error)
        * RIPng (IPv6)
          * IPv6 only
          * Multicast FF02::9
          * VLSM support
      * IGRP
      * EIGRP: Sends triggered updates [not periodically] (if there is a change in network condition then we can notify our neighbors)
        * Uses these elements to calculate metric:
          * Bandiwdth
          * Delay
          * Relaiability
          * Load
          * MTU
  * Link-State: This method chooses the route with the fastest bandwidth (or lowest cost; *see table below*) || It has a map of all the network
    * **OSPF** (Open Shortest Path First): 
      * Uses classeless IP (supports subnetting)
      * Hierarchal structure
    * ISIS
  
  ## EGP Exterior Gateway Protocol
  * Path-Vector: Knows information about the exact path that packets are going to take reach a sepcefic destination network 
    * BGP
  
  ---
  
  ## Administrative Distance (AD)
  If multiple routing protocols exist, the one with the least AD is used
  It can be changed in case of a static address (during the addition of the command)
  
  
  
  |Protocol | Administrative Distance (AD)|
  |-|-|
  |Static interface | 0 |
  |Static IP address | 1 |
  |eBGP | 20 |
  |EIGRP | 90 |
  |OSPF | 110 |
  |RIP | 120 |
  |External EIGRP | 170 |
  |Unknown | 255 |
  
  *Note:*
  External EIGRP is when a route is learnt from a dynamic routing protocol & then redistributed into EIGRP
  
  ---
  
  # Cost table
  **Memorize this** 
  The cost of passing by a link with a
  
  
  | Speed | Cost |
  |-|-|
  | 1Mb/s | 100 |
  | 100Mb/s | 19 |
  | 1Gb/s | 4 |
  | 10Gb/s | 2 |
  
  
  ---
  
  # Default route 
  
  A route that is used to send to networks that aren't explicitly set via either dynamic or static routing
  The default route is 0.0.0.0 with a mask of 0.0.0.0
  **ip route 0.0.0.0   0.0.0.0**
  
  ---
  
  # Mixing routing protocols
  
  If you want to use multiple protocols in 1 router, u have to activate redistribution
  
  ```
  Router(config)#router ospf 1
  Router(config-router)#redistribute static subnets
  ```
  
  ---
  
  # Packet forwarding
  
  * A router contains these components
    * CPU
    * Route cache: Stores packet forwarding information for destination networks. This information is learned when the CPU is used to route a packet to each of those networks.
    * Forwarding Information Base (FIB): Stores L3 routing information that it learns from the IP routing table
    * Adjancy table: Stores information about how to construct the L2 header for the next hop IP address (similar in some way to MAC address table)
  * A router is separated in 2 planes
    * Control plane
    * Data plane
  * Methods of forwarding packets
    * **Process switching**: The CPU get's involved with every forwarded packet
    * **Fast switching**: The route cache is checked to see if a packet destined to a similar network have been sent before, if so the cached data is used. If this is the first packet being sent to this network, the CPU is queried to make a routing decision & stores that in the route cache for futur use.
    * **CISCO Express Forwarding (CEF)**: The CPU populates the routing table to the FIB while the adjancy table is filled with L2 data of neighbors, combining the two the router can rapidly construct the L2/3 headers it needs to make the routing decision 
      * The CEF table can be viewed using `Router#show ip cef`. This command shows us networks, their next hops & egress interfaces
      * The adjacancy table can also be viewed using `Router#show adjacency`
  
  ---
  
  # Passive interface
  
  When configuring a routing protocol, the `Router(config-router)#network <...>` command **DOES NOT** specify the network to be advertised. Instead, it specifies the **network interface** with the following (host/network) address should be participating in this router process. This means that we specify the address (host/network) & the routing process should search for an interface that has an address with the same network address as the one configured & that interface will be participating in that routing process. The routing process will take the network configuration from that interface **NOT** from the command.
  If a network isn't added with that command on that router, the corresponding interface (the one belonging to the same network) **WILL NOT** send routing informations but it **CAN** still receive routing information through it.
  If specifying a single host address (the port address), use a wildcard mask of `0.0.0.0`
  
  A passive interface is an interface that participates in the routing process to take the configuration of the network out of it **but** it doesn't send routing informations
  
  
  
  ---
  
  # Other informations
  
  * Wildcard mask: It's a form of mask used when configuring some routing protocols (eg: OSPF & EIGRP)
    * Calculation: 255.255.255.255 - <mask>  = <wildcard_mask>
  * DHCP relay agent
    * A router that has been configured to forward DHCP discover messages to a specefic destination IP address or specefic destination network
  * The bandwidth of an interface **has to be set correctly** for some calculations (QoS HWQ size for eg)
'''
