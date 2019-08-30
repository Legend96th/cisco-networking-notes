  # Switch
  
  * Link aggregation: active/backup
  * 2 types of switches
    * L2 switches (2xxx series): Basic switch operates on L2 only
    * Multi Layer Switch (MLS): Can reach up to L4 and offer routing capabilities
      * Routed Ports are L3 ports. 
      * Convert a switchport into a routed port with: `no switchport`
  * A MAC address table (or CAM (Content Addressable Memory) ) is used
    * Stores correspondance between MAC addresses and the ports they reside on 
    * Has a limited size
      * If it get's full, frames are flooded (sent to all except the source port)
    * Starts empty
    * If the destination MAC address:
      * Isn't in the MAC table: the frame is flooded (sent to all except the source port) 
      * Is in the MAC table: the frame is sent on the corresponding port
    * Entries of the MAC table are:
      * Added dynamically when a packet is received
      * Romoved: 
        * After an aging time (default 300) 
        * Port state goes down
        * By clearing the table
  
  * After a link is connected to a switchport, STP timers are waited before frames are forwarded or received
  * Port density is the number of ports availiable on a single switch
  * Forwarding rate (sum of all ports speeds): number of ports * speed of port  
  * Sending mechanisms
    * Cut-through  
      * Start sending when receiving 14 bytes
    * Store and forward
      * A full frame must be received to start sending
    * Adaptive Cut-through
      * Start in "Cut-through" mode
      * If an error rate is of 10% change to "store and forward", after a certain timeout if the errors are gone switch back to "Cut-through"
  * ASIC: Application Specefic Integrated Circuitry
    * Circuits that perform switching instead of doing it on the CPU (like bridges did)
    
    
  
  # Notes and [[recommendations]] 
  
  * Manually set the speed (link speed) & duplex (half/full) mode on devices because auto settings might crash [[recommended]]
  * If Full-Duplex is statically configure on an interface it wont participate in auto-configuration 
    * If the other switch is set to auto-config it will pick **half-duplex** instead of full [[warning]]
  * If 2 connected devices have different duplex configurations they wont connect
  
  
  
  
  ---
  
  # Security
  
  ## Defence mechanisms
  
  * **Shutdown unused ports**
    * Shutdown all the ports that are currently unused & only keep the used ones active
  * **NULL_VLAN**
    * Create an unused VLAN 
    * Set all unused ports on it to isolate them
    * Delete that VLAN
  * Non-Default native VLAN
    * Change the default native VLAN on switches to avoid VLAN hopping attacks
  * Port security  
  * Implement 802.1x (an alternative to port security)
  * DHCP Snooping
  * Set a DHCP rate limit to limit the DHCP traffic (packet/sec) received from an interface
    * If the rate limit is violated the port is set to the error disabled mode
  * DAI (Dynamic ARP Inspection)
    
  
  ---
  
  # Port security
  
  * Activate it to prevent Mac address table filling attacks
  * MAC addres can be controlled
    * Dynamically
      * A maximum number of connected MAC addresses
    * Statically
      * A list of authorized MAC addresses
        * Manually configured
        * Dynamically learned the first time
  * Violation actions to be taken after an attack:
    * Protect: Allowed MAC addresses can flow while others are dropped
    * Restrict: Same as protect mode + incrementing the switch's violation counter
    * Shutdown (default): Places the port in "Error Disabled" state and sends an SNMP trap (if the switch is configure for SNMP)
      * To restore the port back in operational mode:
        * Manually shut down & restart the port
        * Use Error Disabled Port Automatic Recovery
  * Port security table is different than mac address table
    * Entries are added by learning dynamically from intering packets MAC address
    * Entries are removed by:
      * **Default** only if a port changes it's **state to down**
      * Configuring port security to use inactivity to remove them
  
  ---
  
  # 802.1x
  
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
  
  ---
  
  # DHCP snooping
  
  * Allows a switch port to reject packets coming in from a DHCP server if that port is set to an untrusted state
  * Uses a database to store the: port, mac address, vlan, ip address
    * By default, if a device is restarted the database is flushed (loss of data)
    * Store the database in a non volatile memory [[recommended]]
  * If a device is using a mac address or ip address that it's not his, it's considered as a violation and the packet is dropped
  
  
  
  ---
  ## Attack types
  
  * VLAN hopping attack
    * An attack where an attaker's device is able to attack a device in another VLAN without crossing over a router
    * The attacker uses double encapsulation of his frames:
      * The packet is encapsulated with the destination's VLAN tag & above that with the native  VLAN tag (mostly using VLAN 1)
      * When the packet reaches the first switch, the native VLAN tag is removed from it & it get's sent on an 802.1Q trunk to the second switch (because there's no need to tag the native VLAN when going on a trunk)  which then removes the VLAN 20 tag & sends it to the destination 
  * DHCP spoofing: An attacker where the attacker has a DHCP server, which respponds with a DHCP Discover message sent from a DHCP client. (NOTE: If the client uses the attacker's DHCP server, it can be convinced that its default gateway is the IP address of one of the attacker's devices)
    
    
  ---
  
  # Error Disabled Port Automatic Recovery
  
  A feature that allows a port that is in error disabled state to come out of that state if the condition cause the port to be in error diasbled state has been resolved 
  
  
  
  --- 
  
  # Link status
  
  * First part (left): Layer 1
  * Second part (right): Layer 2
  
  
  | Layer1                | Layer2             | Link status                                                                                |
  | --------------------- | ------------------ | ------------------------------------------------------------------------------------------ |
  | up                    | up (connected)     | Functional                                                                                 |
  | up                    | down               | Connection issue (L2 keepalives not being received)                                        |
  | down                  | down (not connect) | Cable not connected to at least one switch, or other switch port administrativley shutdown |
  | down                  | down               | L1 interface issue                                                                         |
  | administrativley down | down               | Interface administrativley disabled                                                        |
  
  ---
  
  
  
  | Parameter      | Description                                                                                                                                                                                                                                                                                                                                                     |
  | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | runts          | A frame that is **smaller** that the medium's minimum frame size (eg: 64bytes on an ethernet network) **and has a bad CRC**                                                                                                                                                                                                                                     |
  | giants         | A frame that is larger that the medium's maximum framze size (eg: 1518bytes on a non-jumbo ethernet newtwork) **and has a bad CRC**                                                                                                                                                                                                                             |
  | input errors   | Sum of an interface's errors, including: runts, giants, no buffer, CRC, frame overrun & ignore errors                                                                                                                                                                                                                                                           |
  | output errors  | The total number of errors that prevented a packet from being transmitted                                                                                                                                                                                                                                                                                       |
  | collisions     | The number of times a collision occured (codmmon on shared ethernet networks running in half-duplex mode)                                                                                                                                                                                                                                                       |
  | late collsions | The number of times a collision occured "late" in the transmission (NOTE: "late" means a time greater than the medium's slot time, which is twice the time required for a bit to transit the maximum distance of a medium(eg: later than 51.2 microseceonds for 10Mbps ethernet, 5.12 microseconds for 100Mbps ethernet & 4.096 microseconds for 1Gbps ethernet |
  | CRC            | The number od times the cyclical redundancy check (CRC) value calculated for a frame upon transmission is different than the CRC value calculated for a frame when it is received                                                                                                                                                                               |
  
  
  
  
  
'''
