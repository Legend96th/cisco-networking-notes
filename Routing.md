  # Routing
  
  * Routers stop broadcasts from crossing networks
  * Rules of routing
    * Routers will only add routes to it's routing table with reachable next-hops
    * Routers will only use the best route based on the metric
    * Routes must be belivable
    * Routers will only accept routes that match it's own active protocols
  * Routing sources
    * Connected
    * Static routing
    * Dynamic routing
    * Default route
  * A route is used based on:
    * Lowest AD
    * If same AD then we use the lowest cost
  * Routing types
    * Symetric: The same route is used for the request and the reply
    * Asymetric: A different route is used for the reply
  * Manually set the speed (link speed) & duplex (half/full) mode on devices because auto settings might crash [[recommended]] 
  * Terminology:
    * AS (Autonomous System): Collection of networks all under one administrative authority
      * Routing protocols are divided into 3 categories in how they handle the AS ID
        * Doesn't matter
        * Must match but can be set as we want
        * Is obtained from a higher level authority (ISP, IANA...)
    * AD (Administrative Distance)
  
  ---
  
  # Routing componenets
  ## Administrative Distance (AD)
  * Defines the trustworthiness (priority/preference) of a routing protocol
  * 8-bit numbering system
  * Ranges from 0 - 255
  * If multiple routing protocols exist, the one with the lowest AD is used
  * It can be changed in case of a static address (during the addition of the command)
  * Locally significant value; not distributed to other routers
  
  
  
  | Protocol              | Administrative Distance (AD) |
  | --------------------- | ---------------------------- |
  | Connected             | 0                            |
  | Static route          | 1                            |
  | DHCP Default route    | 2                            |
  | Summerized EIGRP      | 5                            |
  | eBGP                  | 20                           |
  | EIGRP                 | 90                           |
  | OSPF                  | 110                          |
  | IS-IS                 | 115                          |
  | RIP                   | 120                          |
  | External EIGRP        | 170                          |
  | iBGP                  | 200                          |
  | Unknown / Unreachable | 255                          |
  
  *Note:*
  External EIGRP is when a route is learnt from a dynamic routing protocol & then redistributed into EIGRP
  
  ---
  # Metric
  
  * Used for best path selection
  * IGPs use metric for best path calculation
  * Lower value is preferred
  * Depends on the routing protocol
  
  ---
  
  # Static routing
  
  * When adding a static route, the next hop can be:
    * Local interface
    * Remote IP address (IP address of the next hop / other router) [must be on the same subnet of at least 1 interface of a router]
    * Local interface + remote IP address 
  * Floating static routes: 
    * Used as backup routes 
    * A custom AD is set based on the need
  
  
  Notes:
  * Static routes won't show up on the routing table if the next hop IP address is unavailable or the egress interface is down. To make a static route appear on the routing table even if that happend, add the keyword `permanent` at the end of the route addition command [[tip]]
  * If using IPv6, a next hop Link-Local IPv6 address cannot be used on it's own, the egress interface must be specified aswell 
  * If a static route is configure with a local interface on a **multi access link (Ethernet)**, the router will suppose that the configured route **lives on that link** (is on that link) therefore, when a packet is received which needs to be forwarded to that network, an ARP request will be sent **on that link!!** (the link between the 2 routers not the one containing the actual network) which is **wrong!!** [[TIP]]
  
  ---
  
  # Dynamic routing types and classification
  * Routing protocols are classified based on:
    * Neighbor requirements (check if neighbors exist or just send and wait)
    * Route maintenance (is the route valid)
    * Visibility into network topology (full visibility of everyone or just knowing neighbors)
    * Necessity of different data structures (databases, tables...etc)
  * Routing updates type:
    * Incremental / Triggered: When changes are detected they are sent 
    * Full: All of the routing table is sent
    * Periodic: Sent in the specified time interval
      
  
  ## IGP 
  Interior Gateway Protocol
  Provide routing **within** an AS
  
  * Distance Vector: 
    * No neighborships required
    * Resends **all** routes **periodically** (even if there's no TC) to keep track of the routes maintenance 
    * Visibility to directly connected routers only
    * Database of learned routes
    * Examples:
      * RIPv1 & RIPv2 (Routing Information Protocol)
      * IGRP (Interior Gateway Protocol) *deprecated*
  * Link-State: 
    * Neighborships are required
    * Route maintenance
      * Period Hello's 
      * Regenerate LSAs periodically
        * After a **long interval**, a router will resend **only his** LSAs to the entire network
    * Complete topology visibilty **for directly-connected areas**
    * Data structures:
      * LSA database
      * Neighbor table
      * SPF Tree
    * Examples:
      * OSPF (Open Shortest Path First): 
      * ISIS (Intermediate System To Intermediate System)
  * Advanced Distance Vector (Hybrid):
    * Neighborships are required
    * Periodic Hellos between neighbors (route maintenance)
    * Visibility to directly connected routers only
    * Data structures:
      * Topology table of learned routes
      * Neighbor table
    * Examples
      * EIGRP (Enhanced Interior Gateway Protocol)
  
  ## EGP 
  Exterior Gateway Protocol
  Provide routing **between** multiple ASs
  
  * Path-Vector:
    * Neighborships are required
    * Periodic Hellos between neighbors (route maintenance)
    * No knowledge of topology (relies on IGPs for this)
    * Knows information about the exact path that packets are going to take reach a sepcefic destination network 
    * Data structures:
      * -
    * Example:
      * BGP (Border Gateway Protocol)
  
  ---
  # IGP configuration steps
  
  1. Ensure there is at least one functional interface configured with an IP address on the device
  2. Enable the routing protocol
  3. Define networks for which the routing protocol will be active [[recommended]]
      3.1 Use passive interface default
      3.2 Select the interface to participate in the routing protocol
  4. Enable authentication for the routing protocol
  
  
  ---
  
  
  # Default route 
  
  * A route that is used to send traffic destined to networks that have no active route in the routing table
  * Eliminates requiremenets of destination prefix and mask
  * Usually configured for Internet specefic routing (with ISP)
  * Can cause a routing loop if not configured properly [[warning]]
    * If 2 routers are configured with default routes facing eachother
  * The default route is 0.0.0.0 with a mask of 0.0.0.0
    * `ip route 0.0.0.0   0.0.0.0`
  
  ---
  
  # Mixing routing protocols
  
  If you want to use multiple protocols in 1 router, u have to activate redistribution
  
  ```
  Router(config)#router ospf 1
  Router(config-router)#redistribute static subnets
  ```
  
  ---
  
  # Packet forwarding [[important]]
  
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
      * All packets will show with `debug ip packet`
    * **Fast switching**: The route cache is checked to see if a packet destined to a similar network have been sent before, if so the cached data is used. If this is the first packet being sent to this network, the CPU is queried to make a routing decision & stores that in the route cache for futur use.
      * Disable fast switching `Router(conf)#no ip route-cach`; disabling it rolls back to Process switching
      * Only first packet will show with `debug ip packet`
    * **CISCO Express Forwarding (CEF)** (default): *Pre-prepared table*. The CPU populates the **routing table to the FIB** while the **adjancy table is filled with L2** data of neighbors, combining the two the router can rapidly construct the L2/3 headers it needs to make the routing decision 
      * The CEF table can be viewed using `Router#show ip cef`. This command shows us networks, their next hops & egress interfaces
      * Enable CEF `Route(conf)#ip cef`; disabling it rolls back to Fast switching.
      * The adjacancy table can also be viewed using `Router#show adjacency`
      * **Locally generated traffic will never be CEFed** [[important]]
      * **CEFed packets won't show in the `debug ip packet` output because they are not trated by the CPU** [[warning]]
  
  ---
  # Configuring interfaces participating in the routing process
  
  * When configuring a routing protocol, the `#network <...>` command **DOES NOT** specify the network to be advertised. 
  * The router uses the configured address to extract the network that will be matched against it's interfaces addresses. 
  * The routing process will take the network configuration from the interface **NOT** from the routing process's command. 
  * All interfaces matching the network will be participating in the routing protocol:
    * Their network advertised 
    * They'll send and receive routing updates 
  * If an interface's network isn't matched with the configured network, it 
    * Will not send routing informations out of it
    * Can receive routing information
  * If specifying a single host address (the port address), use a wildcard mask of `0.0.0.0`
  
  
  # Passive interface 
  
  * A passive interface is an interface that 
    * Has it's network advertised
    * Doesn't send routing informations out of it
  
  
  
  ---
  
  # Other informations
  
  * Wildcard mask: It's a form of mask used when configuring some routing protocols (eg: OSPF & EIGRP)
    * Calculation: 255.255.255.255 - <mask>  = <wildcard_mask>
  * The bandwidth of an interface **has to be set correctly** for some calculations (QoS HWQ size for eg)
  
'''
