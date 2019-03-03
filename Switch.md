  # Switch
  
  * Link aggregation: active/backup
  * MAC address table starts emptey
  * When a device sends a message it gets registered
  * The message is flooded (broadcasted to all except the source port) to all the devices
  * When a reply is sent, the responder is registered
  * Sending mechanisms
    * Cut-through  
      * Start sending when receiving 14 bytes
    * Store and forward
      * A full frame must be received to start sending
    * Adaptive Cut-through
      * Start in "Cut-through" mode
      * If an error rate is of 10% change to "store and forward", after a certain timeout if the errors are gone switch back to "Cut-through"
  * The entry stays in the mac address table for an aging time that can be checked using 
    * `Switch#show mac address-table aging`
  * The aging time can be changed using
    * `Switch(config)#mac address-table aging-time <>`
  * Entries can be dynamically learnt or statically added using
    * `Switch(config)#mac address-table static <MAC-address> vlan <vlan-num> interface <interface-type> <interface-num>`
  * If the MAC address table is full, the switch will start flooding (sending packets on all ports) the packets that are destined to addresses that are not in it's MAC address table
  * After a link is connected the switch passes by 3 staps (BDUs) 
    * Spanning tree wait time (dont quickly change the root bridge, wait if he may come back)
      * Max age 20s (in case of CST/PVST)
      * or
      * Discarding 1~2s (in case of RPVST/RSTP)
    * Listening 15s
    * Learning 15s
    * Forwarding (as long as link is up)
  * Routed Ports are ports configured to act like a router interface (eg. you can assign the port an IP address). A switch port is converted into a routed port with the interface configuration mode cocmmand: `no switchport`
    
    CAM (Content Addressable Memory) == MAC addresss table
    
    ASIC: Application Specefic Integrated Circuit
    
  ---
  
  **Note**:
  * 2 types of switches
    * L2 switches: basic switch
    * Multi Layer Switch (MLS): can reach up to L4 & do some basic routing
  * POE: Power Over Ethernet
  * Forwarding rates: number of ports * speed of port || sum of all ports speeds
  * Port density is the number of ports availiable on a single switch
  * If 2 connected switches have different duplex configurations they wont connect
    * Can be checked with 
    * `Switch#show interfaces status`
    * `Switch#show interfaces <int>` with one side having a high number of CRC (full duplex side) & the other one having a high number of late collisions (half-duplex side)
  * If Full-Duplex is statically configure on an interface it wont participate in auto-configuration & if the other switch is set to auto-config it will pick half-duplex instead of full 
  
  ---
  
  # Security
  
  ## Defence mechanisms
  
  * **Shutdown unused ports**
    * Shutdown all the ports that are currently unused & only keep the used ones active
  * **Port security**
    * Activate it to prevent Mac address filling attacks
    * Can enter number of possible connected MAC addresses
    * Can set to only be connected to either static or dynamically learned MAC addresses
    * We can configure filtring by Mac address
    * Violation actions to be taken after an attack:
      * Protect: Allowed MAC addresses can flow while others are dropped
      * Restrict: Same as protect mode + incrementing the switch's violation counter
      * Shutdown (default): Places the port in "Error Disabled" state and sends an SNMP trap (if the switch is configure for SNMP)
        * To restore the port back in operational mode:
          * Manually shut down & restart the port
          * Use Error Disabled Port Automatic Recovery
    * View the port secucrity configuration & violation counter with `Switch#show port-security`
    * View the stored MAC addresses `Switch#show port-security address`
  * **NULL_VLAN**
    * Create an unused VLAN & set all unused ports on it to isolate them from prod ports
  * Non-Default native VLAN
    * Change the default native VLAN on switches to avoid VLAN hopping attacks
  * Implement 802.1x
    * No device can communicate to the rest of the network until it authenticates itself
    * Terminology:
      * Supplicant: A device that wants to communicate on the network (end user device)
      * Authenticator: The device that asks the supplicants for authentication (switch for example)
      * Authentication server: The server that checks the authentication credentials of the supplicants & inform the authenticator (Radius server)
    * What happens:
      * When a supplicant is connected to an authenticator, this latter sends a **challenge** to the supplicant asking it to authenticate before gaining access to the network
      * The supplicant then sends it's **username and password** to the authentication server to be checked
      * If the credentials are good, the authentication server sends an **Authorization** to the authenticator informing it that the supplicant is authorized 
    * It can use EAP to encrypt the authentication process (between supplicant and authentication server)
      * EAP (Extensible Authentication Protocol): A protocol, with multiple variants (called "methods") that defines how authentication and encryption are performed between and 802.1x supplicant and authenticator
  * DHCP Snooping: Allows a switch port to reject packets coming in from a DHCP server if that port is set to an untrusted state
  * Set a DHCP rate limit to limit the DHCP traffic (packet/sec) received from an interface
    
  
  
  
  ## Attack types
  
  * VLAN hopping attack
    * An attack where an attaker's device is able to attack a device in another VLAN without crossing over a router
    * The attacker uses double encapsulation of his frames:
      * The packet is encapsulated with the destination's VLAN tag & above that with the native  VLAN tag (mostly using VLAN 1)
      * When the packet reaches the first switch, the native VLAN tag is removed from it & it get's sent on an 802.1Q trunk to the second switch (because there's no need to tag the native VLAN when going on a trunk)  which then removes the VLAN 20 tag & sends it to the destination 
  * DHCP spoofing: An attacker where the attacker has a DHCP server, which respponds with a DHCP Discover message sent from a DHCP client. (NOTE: If the client uses the attacker's DHCP server, it can be convinced that its default gateway is the IP address of one of the attacker's devices)
    
  
  ### Error Disabled Port Automatic Recovery
  
  A feature that allows a port that is in error disabled state to come out of that state if the condition cause the port to be in error diasbled state has been resolved 
  
  * Can be activated for different error causes
    * `Switch(config)#errdisabled recovery cause ?`
  * The time interval can be changed (default 300s = 5min)
    * `Switch(config)#errdisabled recovery interval <sec>`
  * Check the enabled recovery options  
    * `Switch#show errdisabled recovery `
  
  
  --- 
  
  ## Link status
  
  * First part (left): Layer 1
  * Second part (right): Layer 2
  
  
  |Layer1|Layer2|Link status|
  |-|-|-|
  |up|up (connected)|Functional|
  |up|down|Connection issue (L2 keepalives not being received)|
  |down|down (not connect)| Cable not connected to at least one switch, or other switch port administrativley shutdown|
  |down|down|L1 interface issue|
  |administrativley down|down|Interface administrativley disabled|
  
  ---
  
  |Parameter|Description|
  |-|-|
  |runts|A frame that is **smaller** that the medium's minimum frame size (eg: 64bytes on an ethernet network) **and has a bad CRC**|
  |giants|A frame that is larger that the medium's maximum framze size (eg: 1518bytes on a non-jumbo ethernet newtwork) **and has a bad CRC**|
  |input errors|Sum of an interface's errors, including: runts, giants, no buffer, CRC, frame overrun & ignore errors|
  |output errors|The total number of errors that prevented a packet from being transmitted|
  |collisions|The number of times a collision occured (codmmon on shared ethernet networks running in half-duplex mode)|
  |late collsions|The number of times a collision occured "late" in the transmission (NOTE: "late" means a time greater than the medium's slot time, which is twice the time required for a bit to transit the maximum distance of a medium(eg: later than 51.2 microseceonds for 10Mbps ethernet, 5.12 microseconds for 100Mbps ethernet & 4.096 microseconds for 1Gbps ethernet|
  |CRC|The number od times the cyclical redundancy check (CRC) value calculated for a frame upon transmission is different than the CRC value calculated for a frame when it is received|
  
  ---
  
  # Best practises
  
  * Activate port security on all interfaces
  * Activate port fast (STP)
  
  
'''
