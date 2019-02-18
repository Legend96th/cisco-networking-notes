  # EIGRP
  Enhaced Interior Gateway Router Protocol
  
  * IEEE
  * AD = 90
  * Dynamic routing
  * Metrics : Bandwidth, load, reliability, MTU (Maximum Transsmision Unit), delay
  * Sends Hello & updates via multicast 224.0.0.10 every 5s 
  * Hold down time is 15s (3 tries)
  * Unequal load balancing 
  * Symbol: "D"
  * Djstance vector
  * Fast convergence
  * Supports VLSM -summarization
  * Autonomous system
  
  
  --- 
  
  ## EIGRP tables
  
  
  * Neighbor table: directly connected routers
  * Topology table: all routers in the autonomous system
  * Routing table: best route
     
  If 2 or more routes existe the best route's next hop is called the "Successor", the other routers are called "Fasible successors" & are stored in the topology table
  
  ### Neighbor table
  * Next hop
  * Interface
  
  ### Topology table
  * Network
  * FD (Feasible distance): distance from source router to destination router
  * AD (Advertise distance): distance from next hop router to destination router
  * EIGRP nei
