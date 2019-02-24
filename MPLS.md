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
