  # EIGRP
  Enhaced Interior Gateway Router Protocol
  
  * IEEE
  * **L3 protocol** 
    * Protocol number: 88
  * AD = 
    * Internal = 90  
    * External = 170
  * Symbol: "D"
  * Hybrid IGP - Advanced distance vector
  * Fast convergence
  * Partial updates
  * Uses Autonomous systems
  * Classless
    * In the network command, if we don't specify a wildcard mask, the classful mask of that network will be applied
  * Metrics : 
    * Bandwidth
    * Load
    * Reliability
    * MTU (Maximum Transsmision Unit)
    * Delay
  * Multiple network layer protocol support
  * Seamless connectivity accross data-link layer protocol
  * Supports:
    * Automatic and manual summarization
    * MD5 authentication
    * Equal (default) and Unequal load balancing
  * Route poisoning by setting **delay** to the maximum vlaue
  * Terminology:
    * Neighbors
      * Successor: A neighboring EIGRP-speaking router that offers the **best path** to a destination network
      * Successor route: The most attractive route to a destination network that is known to an EIGRP-speaking router
      * Feasible Successor: An EIGRP-neighbor that can get us to a destination network (without causing a routing loop) and acts as a backup to a successor router
      * Feasible Successor route: A loop-free route (known to EIGRP) to a destination network, that acts as a backup to the successor route
    * Distance (Metric)
      * FD (Feasible Distance): metric (distance) from source router to destination router
      * AD (Advertise Distance) / RD (Reported Distance): distance from the neighbor router (next hop) to destination router
  
  
  ---
  # EIGRP packets
  * Updates: 
    * **Most** updates are sent via multicast 224.0.0.10 
      * At first they are sent via multicast
      * A reply is sent in unicast to the updater
      * If a router didn't acknoledge the update, a **unicast** retransmission is sent to him
    * Sends triggered updates [not periodically] 
      * If there is a change in network condition then we can notify our neighbors
  * Neighbor relationships:
    * Sends Hello messages
      * Uses a sequence number to provide **reliability** to some packets
        * In the case of a normal hello, the sequence number = 0
        * If a routing update is sent:
          * A sequence number is used on updates packets
          * An answer to this sequence number must be sent to acknowledge received packets (like TCP)
  * Routing updates:
    * Update (Unicast initially then multicast)
      * *reliable packet*: 
    * Acknowledgments (always unicast)
      * **Not a formal packet**, it's a macanism within a hello message
    * Query
      * *reliable packet*
      * After a route is poisoned by it's source neighbor, a **query** is sent to the other neighbors to check if they have an alternative route to this network
    * Reply
      * *reliable packet*
      * A **reply** is sent to answer to a Query packet
  
  ---
  
  # More about EIGRP packets
  
  * Hello:
    * Flags
      * init
      * conditional
      * restart
      * end of table
    * Sequence
    * Acknowledgment
    * Virtual router id
    * AS
    * Parameters
      * Type params
      * length
      * K1 - K6
      * Holdtime
    * software version
  
  ---
  # Key components
  
  * Neighbor discovery: automatically discover other EIGRP speaking routers residing on attached networks
  * RTP (Reliable Transport Protocol): 
    * Gives EIGRP the ability to guarentee EIGRP packets sent to a neighbor really got there
    * Reliability isn't required for all the EIGRP packets, so reliability is only provided when needed
  * 
  (Diffusing Update Algorithm): An algorith run in an EIGRP speaking router to determine the Successor 
  * Protocol-Dependent Modules
  
  ---
  # Routes categories
  
  * Internal routes
    * Route that was: 
      * Originated withing the AS 
      * With the network command
    * AD = 90
  * External routes
    * Route that was previously learned via some non-EIGRP method and injected into EIGRP with redistribution
    * AD = 170
  
  
  ---
  # Timers
  
  * Types of timers
    * Hello interval
      * Send a hello message to our neighbor
      * Default: 
        * Ethernet: 5 seconds
        * Frame-Relay: 60 seconds
    * Hold Time
      * The hold time doesn't mean that we apply it locally on our interface & that if we don't hear from our neighbor we break the neighborship but instead, it's **sent to our neighbor** telling it to **wait for us during this interval**, and if we don't show up then we break the neighborship
      * Default:
        * Ethernet: 15 seconds
  * Timers don't have to match on neighbors but, it's a best practise to match them
  
  
  
  ---
  # Metric calculation
  
  * EIGRP metric is called "**distance**"
  * Components (Big Dogs Really Like Me):
    * **Bandwidth** (default): (in Kb/s))
    $$BW =  \\frac{10⁷}{\\text{lowest bandwidth on path}} $$ 
    * **Delay** (default): **Sum of delays** spent to exit routers
      * The delay shown with the show command is in microseconds, but in the formula we use (microseconds/10)
    * Relaiability: How reliable our link is
      * A number over 255 (from ## to 255)
      * 255/255: The link is totally reliable
    * Load: How busy our link is
      * A number from 1 to 255
      * 1: less load on the link
    * **MTU**: Not directly used in metric calculation
      * If all other parameters are equal, we use MTU as a tie breaker, the interface with the highest MTU is used
  * Formula
      $$Metric =  256 * \\Bigg[ \\bigg( \\big( K_1*BW \\big) + \\big( \\frac{K_2*BW}{256-Load} \\big) + \\big( K_3*Delay \\big) \\bigg) * \\bigg( \\frac{K_5}{Reliability*K_4} \\bigg) \\Bigg]$$
  * EIGRP K values (**integers** selecting which component enters the Metric calculation and how much does it affect it):
    * **These values need to match on both connected routers to form a neighborship** 
    * Default:
      * K1: 1 (K1 * BW)
      * K2: 0 ((K2 * BW)/(256-load))
      * K3: 1 (K3 * delay)
      * K4: 0 (K5/(K4+reliability))
      * K5: 0 (K5/(K4+reliability))
  
  **Note:**
  K values of Reliability, Load and MTU should be kept with the default values because they are unstable in a prod environment and could lead to the unstability of the network [[recommended]]
  
  
  ---
  # Neighborship conditions
  *Hello sent to 224.0.0.10*
  
  * Same AS (Autonomous System) (1-65536)
  * Same K values on both ends
  * Either fully dynamically discovered or fully statically configured neighbors
  * Authentication credentials (if any)
  * Connectivity
    * No need to be on same subnet. If the neighbor is reachable neighborship can be configured
    * Subnet and subnet mask
      * The Hello messages are sent to 224.0.0.10 with the source address of the router
      * Uppon receiving them, the receiver checks if the source IP address matches his subnet
      * Not an exact matching subnet:
        * Suppose 2 routers with 10.0.0.10/8 and 10.10.10.2/24
        * The router with /8 will accept the hello from the other (address is in the subnet)
        * The router with /24 won't accept the /8 (not in the same subnet)
  * No passive interface on interfaces between neighbors
  
  Notes: 
  * Hello and holdtime don't have to match
    * Hello has to be greater than holdtime (so that the other end will wait; *check up*)
  
  ---
  # Static neighbors
  
  * If we add a neighbor from an interface, other neighbors from that interface will be lost
  * The neighborship has to be statically added on the other neighbor as well
  
  ---
  
  # Feasibility condition 
  *avoid routing loops using the DUAL (Diffusing Update Algorithm) algorithm*
  
  * Evaluate all  Non-Successor routes
    * If Evaluated RD < FD  [AD < FD]
      * Then: the neighbor can be a feasible successor for that path
      * Else: the neighbor can NOT be a fasible successor for that path (possible loop)
  * If a router has an (AD|RD) bigger than the FD of the successor route, a query is sent to it to check if it's generating a routing loop or not
  
  --- 
  
  # Tables
  
  
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
      * EIGRP neighbors
  * Routing table: Best routes
  
  ---
  # Load Balancing
  
  * By default we can configure a maximum of 4 paths
    * Can be up to 16 or 32 according to the platform
  * Variance: A feature that allows EIGRP to load-balance accross a network's successor route and one or more feasible successor routes
    * Variance is an integer (1 - 128) configured in the router configuration
    * Only applies if a route is a **successor route or feasible successor route**
    * The FD of the best route to a network is multiplied by the variance, all feasible routes to that network having a FD **lower** (not equal) than that product will be used in load balancing
    * The best way to find out a value to be configured is dividing the FD of the highest (worst) feasible route to a network by the FD of the successor route to that network and then rounding the number up
  
  ---
  # Things that reset the EIGRP proccess 
  
  * Interface to neighbor going down
  * eigrp kill command
  * clear ip eigrp commands
  * Changing the RID
  * Changing / sending a route summary
  * Changing the metric with an offset-list
  * Neighbor stuck in active (old IOS)
  * Changing / applying a distribute list
    * Deleting the ACL that the distribute-list is referencing can also cause that
  
  
  ---
  # EIGRPv6
  EIGRP routing protocol for IPv6
  
  * Most packets transmitted via multicast FF02::A
  * Same metric formula
  * Utilizes same message types
  * Next-hop is neighbor's Link-Local address
  * Uses IPv6 authentication features
  * No auto summrization
  * Neighbors don't need to be on the same subnet
  * Sends update to multicast FF02::A
  * Loopback interfaces can be used as RIDs even if they are configured in IPv4
  
  
  
'''
linesHighlighted: []
