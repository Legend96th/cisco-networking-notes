content: '''
  # MPLS
  Multiprotocol Label Switching
   
  * Makes fowarding decisions based on a 20-bit label in a 32-bit header
  * Makes forwarding decisions based on labels instead of an IP address
  * Traffic sepparation using different labels
  * Ability to apply QoS to treat different types of traffic differently
  * Copatible with multiple protocols (Frame Relay, ATM...etc)
  
  
  ---
  # MPLS labeling
  
  1. A default IPv4 packet is made of the following [L2 header | L3 header | Payload]. 
  2. MPLS will inject it's header (shim header: 32-bit) in between the L2 and L3 headers.
  3. Inside the shim header is a 20-bit label 
  * Packets are 
    * Labeled when they enter an MPLS cloud
    * The labels are changed inside the MPLS MPLS cloud (to make forwarding decisions)
    * The labels are finally removed when they exit the MPLS cloud
  ---
  # MPLS devices
  
  * CPE (Customer Premise Equipment): A device at a customer site that connects to an MPLS provider
    * The traffic is not labeled at this stage
  * ELSR (Edge Label Switch Router): A device at the edge of an MPLS cloud that adds labels to traffic coming into the cloud and remouves labels from traffic leavingthe cloud
    * Also known as a PE (Provider Edge) router
  * LSR (Label Switch Router): A device inside an MPLS cloud that relabels traffic and makes forwarding decisions based on those labels
    * Also known as a P (Provider) router
    * Labels can be changed at the LSRs
  
  
  ----
  
  
  * Terminology
    * CE: Customer Edge
      * Router of the customer (the one directly conncted to the ISP not internal ones)
    * PE: Provider Edge
      * Router of ISP connecting the customers
      * Translates routing table to MPLS labels & vice-versa
    * P: Provider
      * Router of ISP connecting the PE routers
      * Doesn't understand routing tables
      * Uses MPLS labels
    
  * Each network is tagged with a number (label). Instead of sending all the 32bit ipv4 address (which get checked on each level bit by bit) we send the tag only which will gain us some time
  * MPLS works between layer 2 & 3 (L2.5)
  * PE routers add the MPLS tag (label) to packets received from the CE & sends them to the P
  * Components of MPLS segment
    * Lable: the tagged number
    * Experimental (QoS)
    * S
      * 0: At this level using labels
      * 1: At this level we will start using routing tables (Last router)
    * TTL
  * **Benefits of MPLS:**
    * Traffic engineering: 
      * Load balancing
      * Can block routes
    * MPLS VPN (using VRF)
    * **Fast routing**
