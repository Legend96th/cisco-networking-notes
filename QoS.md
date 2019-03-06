  # QoS
  Quality Of Service (Managed unfairness)
  
  
  
  
  # Converged networks problems
  * Lack of bandwidth
  * End to end delay (fixed & variable)
  * Jitter (variation of delay)
  * Packet loss
  
  ---
   
  # Implementing a QoS policy
  * Identify traffic & it's requirements
  * Divide traffic into classes
  * Define QoS policies for each class
  
  ---
  
  # QoS models
  * Best-effort: No QoS is applied to the packets
  * Integrated services: Applications signal to the network that they require special QoS
  * Differentiated services: The network recognizes classes that require special QoS
  
  ## Best effort
  * No QoS  applied
  * FIFO
  
  ## Integrated services model IntServ - RSVP
  * Similar to a private courrier service 
  * An application reserves the bandwith that it needs (for a certain time) without sharing it with others
  * Some applications have special bandwidth requirments
  * Provides multiple service levels
  * Requests specefic services before sending data
  * Uses RSVP to reserve network resources
  * Uses intelligent queuing mechanisms
  * End to end model
  
  ## Differentiated services model
  * Uses different classes
  * Similar to a package delivery service
  * The network QoS policy enforces differentiated treatment and traffic classes
  * You choose the level of service for each traffic class
  
  Better to use less than 11 classes
  
  ---
  
  # RSVP
  
  The client sends a request to reserve the bandwidth for itself
  
  ---
  
  # Classification
  
  Classification is the identifying and splitting of traffic into different classes for QoS treatment
  * It is the most fundamental QoS building block
  * Traffic can be classified by various means
  * Without classification, all packets are treated the same
  
  ---
  
  # DPI
  Deep Packet inspection
  
  Some apps might try to hide their packets in order to not be identified by the network devices & gets held down. Even by that the devices can perform a deep packet inspection on the first equipement reading the packet (heavy work). The equipement then tags the packet so that the other equipement wont do the same inspection again.
  
  **Note**
  The QoS DPI differs from the one used with IPSs in a way that QoS DPI knows the source & destination of traffic while the IPS doesn't, it just stops malicious traffic from going in to the network using signatures.
  
  # NBAR
  Network Base Application Recognition
  
  NBAR is a protocol and application discovery mechanism that does the following
  * Classifies traffic by protocol (L4 - L7)
  * Uses the same principal of identifying traffic based on signature (same as antivirus) & behaviour
  * Performs stateful & dynamic inspection
  * Operates in two methodes:
    * Active: MQC classification
    * Passive: Protocol discovery
  * Can classify applications that use the following:
    * Statically assigned TCP & UDP port numbers
    * Non-UDP & non-TCP IP protocols
    * Dynamically assigned TCP & UDP port numbers that are negotiated during connection establishment (requires stateful inspection)
  * Can perform subport classification or classification based on deep packet inspection
  
  ---
  
  # Marking
  
  * Marking is used to mark each packet as a member of traffic class so that packets can be easily identified throughout the network
  * Marking is also known as coloring, marks each packet as a member of a network class so that the packet class can be quickly recognized throughout the restt of the network
  * The marking field available at the L2 (data-link layer) is dependent on the protocol in use and is bound by the extent of the L2 domain
    * CoS (Class Of Service) (3 Bits) : 8 classes from 0 to 7
      * Classes 6 and 7 are reserved for network use 
    * Each data link protocol has its own marking mechanism
      * Ethernet 802.1Q: Uses the PRI (priority) field as the CoS Bits 
        * The PRI field is in the TCI Bytes (Tag Control Information)
      * 802.1P: Some NICs are able to add a 4 Byte header simillar to the 802.1Q header in-order to use the PRI bits with an exception that all the "VLAN ID" bits are set to 0
      * MPLS: Uses the EXP bits (Experimental bits) as the CoS bits 
      * ISL frame: Uses the VLAN bits in the ISL header as the CoS bits 
    * The CoS marking gets rewritten when it crosses a router 
  * At the L3 (network layer), the ToS Byte (Type Of Service) [IPv4] or TC Byte (Traffic Class) (IPv6) is used to mark the traffic class of packets and can be preserved across the entire network domain
    * IP precedence bits (3 bits from the ToS Byte)
    * DSCP field bits (6 bits from the ToS Byte)
  * In order to provide end-to-end QoS accross a network, QoS markings must be mapped between data link and network marking fields
  * QoS service classes are used to group packets that are to receive similar levels of network QoS. These service classes are used to create a network-wide QoS policy
  
  ---
  
  # Mapping QoS marking between OSI layers
  
  * ToS is not used when the IP header is encapsulated in an ethernet frame
  * ToS bits can be mapped to CoS and vice versa
  * ToS bits can also be mapped to MPLS EXP bits and vice versa
  
  ---
  
  # Traffic policing & shaping
  
  * These mechanisms must classify packets before policing or shaping the traffic rate
  * Traffic shaping queues excess packets to stay within the desired traffic rate
  * Traffic policing typically drops or marks excess traffic to stay within a traffic rate limit
  * Policier
    * Trasmit, drop or mark, then transmit the packet
    * Drops the packets if the limit is exceeded
    * Incoming and outgoing directions
    * **Drops** out-of-profit packets 
    * Dropping causes TCP retransmits
    * Supports packet marking or re-marking
    * Less buffer usage (shaping requires an additional shaping queuing system)
    * **Used on lower speed interfaces**
  * Shapper
    * Buffer exceeding packets
    * Lowers the bandwidth if the limit is exceeded
    * Outgoing direction only
    * Queuing out-of-profit packets until a buffer gets full
    * Buffering minimizes TCP retransmits
    * Does not support marking or re-marking
    * Supports interaction with Frame Relay congestion indication
    * **Used on higher speed interfaces**
  
  ## How does speed limiting work
  
  When we send on a line, **we send at a constant speed**. By limiting traffic bandwidth of traffic, we are not limiting the traffic speed at an exact instant (we send at a constant speed) but instead **we limit the average speed** that we send with.
  Metaphorically, there's a bucket that has a certain capacity. This bucket doesn't get emptied (sent) unless it's full. When that bucket gets sent, it's sent using the line speed. On a specified time interval, we place a certain amount of data in that bucket in a way that it get's full to answer the average speed that we want 
  
  * Terminology:
    * CIR (Committed Information Rate): (Avg_speed/s) Averge  speed over the period of a second
    * Bc (Committed Burst): Number of Bits (for shaping) or Bytes (for policing) that are deposited in the token bucket during a time interval
    * Tc  (Timing Interval): The interval at which tokens are deposited in the token bucket
  * **CIR = Bc / Tc**
  
  ---
  # Single rate & multiple coloring
  
  * Single rate 2 colors: If a class exceeds it's limit, mark it as a lower priority class, if it exceeds again drop it
    * Warning: Avoid having recursive re-coloring
      * Class A gets remarked as B, B gets remarked as C ...etc; If A sends too much traffic, it'll use all the traffic thus making the QoS inevident
  
  ---
  ---
  
  # Congestion management
  
  * Queuing algorithms
    * FIFO
    * PQ
    * Round Robin
    * WRR
    * DRR
    * WFQ
    * CBWFQ
    * LLQ
  
  
  ### Queuing algorithms
  * **FIFO**
    * First packet in is first packet out
    * Simplest of all algorithms with a single queue
    * All individual queues are FIFO
  * **Priority queuing**
    * Multiple queues
    * Allows prioritization 
    * Always empties first queue before going to the next queue 
      * Empty Queue 1
      * If Q1 is empty, then dispatch one packet from Q2
      * If both Q1 & Q2 are empty, then dispatch one packet from Q3
      * ....
    * If a packet is in the last queues it may stay there forever
  * **Round-Robin queuing**
    * Allows prioritization
    * Implements round-robin between all the queues
  * **Weighted Round-Robin (WRR)**
    * Allows prioritization
    * Asign a weight to each queue
    * Dispatches packets from each queue proportionally to the assigned queue
    * Some implementations of WRR dispatch a configurable number of bytes (threshold) from each queue for each round-several packets can be sent in each turn
    * The router is allowed to send the entire packet even if the sum of all bytes is more than the threshold
      * If the threshold is set to (3K )
    * **If a higher priority queues contains smaller size packets it will take it more time to send the less priority & wait**
  * **Defecite Round Robin**
    * Solves problem with some implementations of WRR
    * Keeps track of the number of "extra" bytes that are dispatched in each round-the "defecit"
    * Adds the "deficit" to the number of bytes dispatched in the next round
    * Problem resolved with DRR
      * Threshhold of 3 Kbit
      * 
  * **Wheited Fair Queuing (WFQ)**
    * Classification
      * A fixed number of per-flow queues is configured
      * The hash function is used to translate flow parameters into a queue number
      * Packets of the same flow end up in the same queue
      * System packets of same flow end up in the same queue
      * System packets (eight queue) and RSVP flows (if configured) are mapped into seperate queues
      * It is important to note that the number of queues configured has to be larger than the expected number of flows
      * Parameters:
        * Sorce IP address
        * Destination IP address
        * Source TCP or UDP port
        * Destination TCP or UDP port
        * Transparent protocol
        * ToS field
      * A hash algorithm is used to produce the index of the queue in which the packet is enqueued
  * **Class-Based Wheited Fair Queuing (CBWFQ)**
    * CBWFQ is a mechanism that is used to **guarentee bandwidth** to classes
    * CBWFQ extends the standard WFQ functionality to provide suppprt for user-defined traffic classes 
      * Classes are based on user-defined match criteria
      * Packets satisfying the match criteria for a class constitute the traffic for that class
    * A queue is reserved for each class and traffic belonging to a class is directed to that class queue 
  * **Low Latency Queue (LLQ)**
  
  ---
  
  # Queuing components
  
  
  ### Hardware Queue (HWQ)
  * FIFO only (no diff in priority)
  * Can be resized by admin
    * Keep it small to force the congestion in HWQ so that the SWQ is used & QoS is implemented
  
  #### HWQ size
  * **Routers determine the size of HWQ based on the configured bandwidth of the interface**
  * The length of the HWQ can be adjusted using `tx-ring-limit` command
  * Reducing the size of the transmit ring has 2 benefits
    * It reduces the max amount of time that packets wait in the FIFO quque before being transmitted
    * It accelerates the use of QoS in the CISCO IOS software
  * Improperoper tuning of the HWQ may produce undesirable results:
    * Long TxQ may result in poor performance of the software queue
    * Short TxQ may result in a large number of interrupts which cause high CPU utilization and low link utilization
  
  ### Software Queue (SWQ)
  * Implements difference in priority
  * Can be selected and configured depending on the platform and Cisco IOS software version
  
  
  if(Hardware_Queue full){
    place it in SWQ
  } else {
    place it in HWQ
  }
  
  --- 
  
  # Managing congestion with tail drop
  
  * The router interfaces experience congestion when the output queue is full
    * Additional incoming packets are tail-dropped
    * Dropped packets may cause significant application performance degradation
  * Tail drop should be avoided because it contains significant flaws
    * TCP synchronization
    * TCP starvation
    * No differentiated drop (important packets get dropped aswell)
  
  ---
  
  # Congestion avoidance 
  
  * Tail drop can be avoided if congestion is prevented
  * Tail drop is the default queuing response to congest and has several negative impacts on the network performance
  * Tail drop can cause TCP synchronization, TCP starvation, delay and jitter
  
  
  ## Random Early Detection (RED)
  * RED is a mechanism that randomly drops packets before a queue is full
  * RED increases the drop rate as the average queue size increases
  * RED results in the follwing
    * TCP sessions slow down (reduce the window size) to the approximate rate of output-link bandwidth
    * The average queue size is small (much less than the maximum queu size)
    * TCP sessions are desynchronized by random drops
  
  RED: Randomly drops packets from a queue based on priority & drop probability. Can be set with different settings & different aggressiveness 
  
  
  ## Wheighted Early Detection (WRED)
  
  * WRED profiles can be manually set
  * WRED has 8 default value sets for IP precedence-based WRED
  * WRED has 64 default value sets for DSCP-based WRED
  
  ---
  # DSCP
  
  * DSCP values: 
    * IETF selected 21 default categories called PHBs (Per Hop Behaviors)
    * Last digit is always 0
    * 4 categories
      * Default: 
        * Decimal value: 00
        * Binary value: 000000
      * EF (Expedited Forwarding): 
        * Highest priority traffic
        * Decimal value: 46
        * Binary value: 10111**0**
      * CS (Class Selector): 100% backwards compatibility with IP precedence (the last 3 bits to the right are all set to 0)
        * CS1
        * CS2
        * CS3: 
          * Decimal: 24
          * Binary: 011000
        * CS4
        * CS5
        * CS6: RESERVED
        * CS7: RESERVED
      * AF (Assured Forwarding)
        * Uses 2 key concepts
          * Class: 
            * IP Precedence equivelent value
            * Selected based on the 3 **left** most bits
            * 4 possible classes:
              * Class 1: 001
              * Class 2: 010
              * Class 3: 011
              * Class 4: 100
          * Drop probability
            * If WRED is enabled, packets with the highest DP are dropped first **no matter what class the traffic is** 
            * Selected based on the 3 **right** most bits
            * 3 values
              * Low DP (1): 01**0**
              * Medium DP (2): 10**0**
              * High DP (3): 11**0**
        * Naming convention: AF\\<class_num\\>\\<drop_probability_number\\>
          * Example: AF12 - AF42 - AF43 - AF11
  ---
  
  # ECN
  Explicit Congestion Notification
  
  * The ECN uses the last 2 right most bits of the ToS byte 
  * The bits are as follows: [...|ECT bit|CE bit]
    * ECT bit: ECN-capable transport
    * CE: Congestion experienced
  * Values:
    * 00: Router is not ECN capable
    * 01: Router is ECN capable but is not experiencing congestion
    * 10: Router is ECN capable but is not experiencing congestion
    * 11: Router is ECN capable and is currently experiencing congestion
  
  
  ---
  
  ## DSCP encoding 
  * **DS field**: The IPv4 header ToS (Type of Service) Byte or the IPv6 TC (traffic class) Byte when interepted per definition that is given in RFC2474
  * **DSCP**: The first six bits of the DS field that are used to select a PHB
  
  ## DS field drop mechanism
  If we use DS field method (1st), the buffer can be full of lower priority packets & the equipement will drop higher priority packets from coming in
  
  ## Assured forwarding
  The AF PHB does the following
  * Guarentees bandwidth and allows 
  
  ## AF drop probability (DSCP)
  * Each AF class uses three DSCP values
  * Each class is independantly forwarded with guarenteed bandwidth and congestion avoidance
  
  
  
  ---
  # Link fragmentation & interleaving 
  
  Divide big data packets to smaller ones & send small VoIP packets in between them.
  Use it only with connexions slower than **768kb/s**
  
  ---
  # RTP header compression
  *Used in VoIP*
  
  VoIP uses a huge amount of extra small packets.
  Since the data contained in the VoIP packets is 3 times smaller than the header & since the header is repeated in all the packets of the same conversation, RTP header compression uses a small identifier between routers to identify the same conversations. It sends the header only once & then sends all the voice packets containing only the ID that was chosen for it. Uppon arrival, the router re-attaches the header back to each packet & sends it to the LAN.
  
  ---
  # MQC (Modular QoS CLI)
    * class-map: 
      * no more than 11 classes
      * match-all (default): All the match criteria have to be satisfied in order for the traffic to be matched into that class 
      * match-any: Only one match criteria need to be satisfied in order for the traffic to be matched into that class 
    * policy-map
      * Assign a policy to a class
    * service-policy
      * Some policies can only be applied on a single traffic flow direction (inbound or outbound)
    * interface
  
  ---
  
  # Configuration
  
  * Enable QoS on a switch
  ```
  Switch(config)#mls qos
  ```
  
  * Trust an interface's marking values
  ```
  # Set the type of marking to be trusted from that interface
  Switch(config-if)#mls qos trust (cos|dscp|ip-precedence)
  
  # Set the type of device to be trusted from that interface (CDP must be enabled for this to work)
  Switch(config-if)#mls qos trust device (cisco-phone|cts|ip-camera)
  ```
  
  
  
  
  
  * Applying MQC
    * Class-map
    ```
    Router(config)#class-map [match-(all|any)] <class_name>
    Router(config-cmap)#match ?
    ```
    
    * Policy-map
    ```
    ## Create a policy map
    Router(config)#policy-map <policy_name>
    
    ## Add a class to the policy
    Router(config-pmap)#class <class_name>
    
    ### Apply a marking to a class
    Router(config-pmap-c)#set dscp <dscp_value>
    
    {
    ### Set a minimum bandwidth guarenteed to a class
    Router(config-pmap-c)#bandwidth <bw_k-bit-ps>
    OR
    ### Set a maximum bandwidth to a class
    Router(config-pmap-c)#police <bw_bits-ps> [...]
    OR
    ### Set a priority class (the speed here is a limit not a guarenteed speed)
    Router(config-pmap-c)#priority <bw_k-bit-ps>
    }
    ### Setup RED/WRED
    Router(config-pmap-c)#random-detect dscp-based
    
    ### Setep ECN
    Router(config-pmap-c)#random-detect ecn
    
    ### Setup RTP header compression on low speed interfaces
    Router(config-pmap-c)#compression header ip rtp
    ```
    
    * Service policy
    ```
    Router(config-if)#service-policy (output|input) <policy_name>
    ```
    
  * Show mapped classes
  ```
  Router#show class-map
  ```
   
  * Show policies associated with classes
  ```
  Router#show policy-map [interface <interface>]
  ```
  
  * *other*
  
  ```
  Router(config)#class-map match-any TestMedea
  Router(config-cmap)#match protocol attribute category <category> <sub-category>
  
  ```
  
  ```
  policy-map match-all
  ```
  
  * NBAR2
  ```
  class-map
  ```
'''
