  # EIGRP
  Enhaced Interior Gateway Router Protocol
  
  * IEEE
  * AD = 90
  * Dynamic routing
  * Metrics : Bandwidth, load, reliability, MTU (Maximum Transsmision Unit), delay
  * Sends updates via multicast 224.0.0.10 
  * Sends Hello messages (Default: every 5s)
  * Default Hold down time is 15s (3 tries)
  * Equal (default) and Unequal load balancing 
    * By default we can configure a maximum of 4 paths
    * Variance: A feature that allows EIGRP to load-balance accross a network's successor route and one or more feasible successor routes
      * Variance is an integer configured in the router configuration
      * Only applies if a route is a successor route or feasible successor route
      * The FD of the best route to a network is multiplied by the variance, all feasible routes to that network having a FD lower or equal to that product will be used in load balancing
      * The best way to find out a value to be configured is dividing the FD of the highest (worst) feasible route to a network by the FD of the successor route to that network and then rounding the number up
  * Symbol: "D"
  * Djstance vector
  * Fast convergence
  * Autonomous system
  * Supports VLSM -summarization
  * Classless
    * In the network command, if we don't specify a wildcard mask, the classful mask of that network will be applied
  * Terminology:
    * Succesor: A neighboring EIGRP-speaking router that offers the best path to a destination network
    * Succesor Route: The most attractive route to a destination network that is known to an EIGRP-speaking router
    * Feasible Successor: An EIGRP-neighbor that can get us to a destination network (without causing a routing loop) and acts as a backup to a successor router
    * Feasible Successor Route: A loop-free route (known to EIGRP) to a destination network, that acts as a backup to the successor route
  
  
  ---
  # EIGRP key components
  
  * Neighbor discovery: automatically discover other EIGRP speaking routers residing on attached networks
  * RTP (Reliable Transport Protocol): 
    * Gives EIGRP the ability to guarentee EIGRP packets sent to a neighbor really got there
    * Reliability isn't required for all the EIGRP packets, so reliability is only provided when needed
  * DUAL (Diffusing Update Algorithm): An algorith run in an EIGRP speaking router to determine the Successor 
  * Protocol-Dependent Modules
  
  ---
  # EIGRP timers
  
  * Types of timers
    * Hello interval
      * Send a hello message to our neighbor
      * Default: 
        * LAN: 5 seconds
        * Frame-Relay: 60 seconds
    * Hold Time
      * The hold time doesn't mean that we apply it locally on our interface & that if we don't hear from our neighbor we break the neighborship but instead, it's **sent to our neighbor** telling it to **wait for us during this interval**, and if we don't show up then we break the neighborship
      * Default:
        * LAN: 15 seconds
  * Timers don't have to match on neighbors but, it's a best practise to match them
  
  
  
  ---
  # Metric calculation
  
  * Components (Big Dogs Really Like Me):
    * **Bandwidth** (default): BW = (10⁷ / least-bandwidth (in Kb/s))
    * **Delay** (default): Sum of delays spent to exit routers
      * The delay shown with the show command is in microseconds, but in the formula we use (microseconds/10)
    * Relaiability: How reliable our link is
      * A number over 255 (from ## to 255)
      * 255/255: The link is totally reliable
    * Load: How busy our link is
      * A number from 1 to 255
      * 1: less load on the link
    * MTU: Not directly used in metric calculation
      * If all other parameters are equal, we use MTU as a tie breaker, the interface with the highest MTU is used
  * EIGRP K values (**integers** selecting which component enters the Metric calculation and how much does it affect it):
    * **These values need to match on both connected routers to form a neighborship** 
    * Default:
      * K1: 1 (K1 * BW)
      * K2: 0 ((K2 * BW)/(256-load))
      * K3: 1 (K3 * delay*)
      * K4: 0 (K5/(K4+reliability))
      * K5: 0 (K5/(K4+reliability))
  
  
  
  --- 
  
  # EIGRP tables
  
  
  * Neighbor table: Contains information about EIGRP router speaking neighbors (directly connected routers)
    * Contents:
      * Autonomous system
      * Neighbor address
      * Interface
      * Hold uptime
  * Interface table: Contains a list of interfaces that are participating in EIGRP
    * Contents:
      * Autonomous system
      * Interface
      * Peers
  * Topology table: Contains all routes known in the EIGRP autonomous system
    * "Fasible successors" are stored in the topology table
    * Contents:
      * Network
      * (FD / AD)
        * FD (Feasible Distance): metric (distance) from source router to destination router
        * AD (Advertise Distance): distance from the neighbor router (next hop) to destination router
      * EIGRP neighbors
  * Routing table: Best routes
  
  ---
  # EIGRP neighborship conditions
  
  * Same AS (Autonomous System)
  * Same K values on both ends
  * Either fully dynamically discovered or fully statically configured neighbors
  * Authentication credentials (if any)
  * Subnet and subnet mask
  * No passive interface on interfaces between neighbors
  
  ---
  
  # EIGRP feasibility condition
  
  * An EIGRP route is a feasible successor route if its reported (Advertised) distance (RD or AD) from our neighbor is less than the feasible distance (FD) of the successor route (the FD of the best route)
    * (RD|AD) < FD 
  * If a router has an (AD|RD) bigger than the FD of the successor route, a query is sent to it to check if it's generating a routing loop or not
  
  ---
  # EIGRPv6
  EIGRP routing protocol for IPv6
  
  * Next-hop is neighbor's Link-Local address
  * Uses IPv6 authentication features
  * No auto summrization
  * Neighbors don't need to be on the same subnet
  * Sends update to multicast FF02::A
  * Loopback interfaces can be used as RIDs even if they are configured in IPv4
  
  
  ---
  # Commands 
  
  ## EIGRP
  
  * Change the EIGRP timers of an interface
  ```
  Router(config-if)#ip hello-interval eigrp <autonomous_system> <time_in_seconds>
  Router(config-if)#ip hold-time eigrp <autonomous_system> <time_in_seconds>
  ```
  
  * Change the K values (0-255)
  ```
  Router(config-router)# metric weights 0 (K1) (K2) (K3) (K4) (K5) 
  ```
  
  * Configure an EIGRP autonomous system routing
  ```
  # The AS must match on routers in order to form a neighborship
  Router(config)#router eigrp <autonomous_system>
  
  # Add interfaces to participate in the routing autonomous system
  Router(config-router)#network <net_ip> <wildcard_mask>
  ```
  
  * Statically add neighbors
    * If we add a neighbor from an interface, other neighbors from that interface will be lost
    * The neighborship has to be statically added on the other neighbor as well
  ```
  Router(config-router)#neighbor <neighibor_ip_add> <interface>
  ```
  
  * Change maximum load balancing paths
  ```
  Router(config-router)#maximum-paths <nbr>
  ```
  
  * Configure the variance (load balancing across unequal paths)
  ```
  Router(config-router)#variance <variance_value>
  ```
  
  * Show the (neighbors|interface|topology) table
  ```
  Router#show ip eigrp (neighbors|interfaces|topology [all-links])
  ```
  
  * Show eigrp timers
  ```
  Router#show ip eigrp interface detail
  ```
  
  * Show K values
  ```
  Router#show ip protocols
  ```
  
  ## EIGPv6
  
  * 
  ```
  # Enable IPv6 routing
  Router(config)#ipv6 unicast-routing
  
  # Add a routing autonomous system
  Router(config)#ipv6 router eigrp <autonomous_system>
  
  # 
  Router(config-rtr)#
  
  # Add interfaces to participate in a routing autonomous system
  Router(config-if)#ipv6 eigrp 1
  ```
  
  * Show if a route to a network exists
  ```
  Router#show ipv6 route <network_add>/<mask>
  ```
  
  * Show K values, RID
  ```
  Router#show ipv6 protocols
  ```
  
  * Show the hello interval, hold time
  ```
  Router#show ipv6 interfaces detail
  ```
  
'''
linesHighlighted: []
