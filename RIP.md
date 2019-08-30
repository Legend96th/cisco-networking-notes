content: '''
  # RIP 
  Routing Information Protocol
  
  * IEEE 
  * L5 protocol
    * Uses UDP on L4
  * Metric: Hop count
  * Load balancing (max of 6) is used for equal metric routes
  * Exchange the **entire** routing table (address + number of hops) 
    * Every **30 secs** (full update)
    * When a change in the topology occurs (triggered updates)
  * The max hops: 15 (16 router passed)
  * Uses the poison reverse rule: If a router knows that one of his directly connected routes is unavailiable, it sends an update with a metric of 16 to inform other routers that it no longer exists
  * Uses the split horizon rule: If a route is received from a router, it is not sent back to it with the routing table
  * Uses route summerization by default (can lead to some errors). 
    * The summerization is class based (the route summary will be a classful route not classless) [[tip]]
  * **All RIP versions** use classeful addressing when choosing the networks that participate in the routing protocol
    1. 2 interface are in 10.1.1.0/24 and 10.2.2.0/24
    2. RIP is configured with **either** (only one of) 10.1.1.0/24 or 10.2.2.0/24
    3. RIP will change the configured network back to it's classful base (10.0.0.0/8)
    4. **Both interfaces** will participate in the routing protocol
  
  ## Notes
  * The path with the shortest hop count maybe he slowest
  
  ---
  
  # RIPv1:
    * Classful routing only (**NO subnetting**)
    * Sends **updates in broadcast** 
    * Timers:
      * Update timer: 30sec
      * Hold down timer: 180sec [The directly connected link becomes unavailable]
      * Invalid timer: **180sec** (30+150) [The routes received from a router that hasn't sent updates in 180sec are **considered unusable**]
            * Flush timer: **240sec** (180+60) [The routes received from a router that hasn't sent updates in 240sec are **removed**]
  
  # RIPv2
    * Sends **updates in multicast 224.0.0.9**
    * Only RIPv2 received update
    * Supports VLSM
      * VLSM is supported while broadcasting the network not when choosing which network will participate in the routing protocol [[warning]]
      * If using subnetting, **auto summary must be disabled** to avoid having errors [[recommended]]
        * If multiple routes can be summarized but they are not exiting the same interface, it'll genrate an error
  
  
  # RIPng (IPv6)
    * IPv6 only
    * Multicast FF02::9
    * VLSM support
      * VLSM is supported while broadcasting the network not when choosing which network will participate in the routing protocol [[warning]]
