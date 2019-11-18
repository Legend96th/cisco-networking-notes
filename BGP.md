content: '''
  # BGP
  Border Gateway Protocol
  aaa
  * EGP
  * Variants
    * EBGP: EGP between various ASs
    * IBGP: EGP in the same AS
  * Path vector
    * Counts the number of autonomous system hops 
  * Application
    * Port TCP/179
  * Forms neighborships
    * No need to be adjacent to be neighbors with a router
    * **Neighbors** can't be dynamically learnt, the **must be statically added**
  * A TCP session is established between neighbors
  * Advertises address prefix and length 
    * Called NLRI (Network Layer Reachability Information)
  * Advertises a collection of path attributes (PAs) that can be used for path selection
  * BGP isn't as fast as IGPs but it can advertise more routes between neighbors
  * AS range (16 bit, the 32 bit is not included):
    * Public: 1 - 64495
    * Private: 65512 - 65534
    * Reserved: 0, 64496 - 65511, 65535
  * AS_PATH: A BGP path attribute that contains the list of autonomous systems into which packets must flow in order to reach a specified destination network, where BGP prefers shorter AS paths
    * An BGP router updates the AS_PATH when advertising to an eBGP peer
    * An BGP router doesn't update the AS_PATH when advertising to an iBGP peer
  * No load balancing by default (there are many checks that it's highly improbable that 2 routes will match all criterias in the same way)
  * In order to make a path less desirable for BGP, we can advertise that in order to go to a certain AS  you have to pass by that AS multiple times thus making BGP select another path
  * Local preference: A BGP attribute that can be assigned to routes received from another AS and exchanged between routers in an AS, where BGP prefers routes with a higher Locel Preference 
  * Unlike other routing protocols, the BGP `network` command is used to specify a network to be advertised (not select an interface participating in a routing process)
  * Authentication information is sent in TCP option of the TCP header not the BGP data
  * We use BGP in three cases
    * An AS is multihomed
    * An AS is a transit AS
    * Inter-AS routing policy must be manipulated
  * We DON'T use BGP in three cases:
    * The AS is a single-homed
    * Memory and processor ressources are insufficient
    * There is insufficient understanding of BGP route filtering and unavailable path selection
    * When there is no need to manipulate your routing policy
  
  iBGP is used between edge routers and internal routers that have interconnexion between ebpg routers
  
  ---
  # Neighbors
  
  * Called peers
  * Static neighborships **only**
  * By default assumed to be directly connected (can cause problems for indirectly connected neighbors)
  
  # eBGP neighborship requirement    
  
  * Statically configure the neighbor
  * Correct IP address
  * Correct AS numbers on both ends
  * Neighbors must be reachable via an IGP (or static) route 
  * Different Router-ID
  * Authentication
  * Complete TCP connection between neighbors
  * No ACL blocking TCP/179
  * The source IP address used to reach a peer must match the peer's 'network' command
  
  ---
  
  # Message types
  
  ---
  
  # Tables
  * BGP neighbor table
  * BGP table
  * routing table
  
  ---
  
  # Configuration
  
  * Setup a BGP routing process
  ```
  # Create a routing process
  Router(config)#router bgp <AS_id>
  
  # Add neighbors
  Router(config-router)#neighbor <ip_add> remote-as <neighbor_as>
  
  # Select network to be advertised; unlike other routing protocol configuration, the specified network is advertised not an interface belonging to his network
  Router(config)#network <ip_address> mask <mask>
  ```
  
  * Show the BGP routing table (not the router's routing table)
  ```
