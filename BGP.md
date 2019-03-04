content: '''
  # BGP
  Border Gateway Protocol
  
  * EGP
  * Path vector
    * Counts the number of autonomous system hops 
  * Forms neighborships
    * No need to be adjacent to be neighbors with a router
    * **Neighbors** can't be dynamically learnt, the **must be statically added**
  * A TCP session is established between neighbors
  * Advertises address prefix and length 
    * Called NLRI (Network Layer Reachability Information)
  * Advertises a collection of path attributes (PAs) that can be used for path selection
  * AS_PATH: A BGP path attribute that contains the list of autonomous systems into which packets must flow in order to reach a specified destination network, where BGP prefers shorter AS paths
  * In order to make a path less desirable for BGP, we can advertise that in order to go to a certain AS  you have to pass by that AS multiple times thus making BGP select another path
  * Local preference: A BGP attribute that can be assigned to routes received from another AS and exchanged between routers in an AS, where BGP prefers routes with a higher Locel Preference 
  * Variants
    * EBGP: EGP between various ASs
    * IBGP: EGP in the same AS
  * Unlike other routing protocols, the BGP `network` command is used to specify a network to be advertised (not select an interface participating in a routing process)
  
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
