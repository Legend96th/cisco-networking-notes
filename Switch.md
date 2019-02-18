  # Switch
  
  * Link aggregation: active/backup
  * MAC address table starts emptey
  * When a device sends a message it gets registered
  * The message is flooded (similar to broadcasted) to all the devices
  * When a reply is sent, the responder is registered
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
  
  ## Security
  
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
