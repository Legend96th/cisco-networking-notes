  # STP
  Spanning Tree Protocol
  
  * Spanning tree types (versions)
    * CST (Common Spanning Tree) [IEEE 802.1D] 
    * PVST (Per VLAN Spanning Tree) [cisco] 
    * **PVST+** [cisco]  
    * RSTP (Rapid Spanning Tree Protocol) [IEEE 802.1W] 
    * **RPVST** (Rapid PVST+) [cisco]
    * MISTP (Multiple Instances Spanning Tree Protocol) [cisco]
    * **MSTP** (Multiple Spanning Trees Protocol) [IEEE 802.1S]
  * Types of devices
    * <non root??>
    * Root bridge: An STP topology has a single root bridge. The bridge (or switch) with the lowest bridge ID (BID) is elected as the root bridge
      * Composition: BP|MAC
        * Bridge priority (0-61440), default: 32768
        * MAC address
      * The election of a root bridge is started when we don't receive BPDUs (Root Bridge not existing or went down)
  * Port types
    * P2P: Switch to (Switch|PC|Router)
      * Edge-portfast: A special case that is applied only when we have **point-to-point and portfast** enabled 
    * Shared: Switch to Hub
  * Port states
  | Port state          | Description                                                                        |
  | ------------------- | ---------------------------------------------------------------------------------- |
  | Root port           | The port on a non-root bridge that is closest to the root bridge, in terms of cost |
  | Designated port     | The port on a network segment that is closes to the root bridge, in terms of cost  |
  | Non Designated port | Ports that block traffic, in order to preserve a loop-free L2 topology             |
  | Disabled port       | A port that is administratively shut down                                          |
   
  
  ---
  
  # **Traditional**
  * Topology change notification
    * The switch experiencing the topology change sens a Topology Change Notification (TCN) to the Root Bridge, which then sends a Topology Change Acknoledgment (TCA) to the switch reporting the change, and then the Root Bridge notifies other switches of the change by setting the Topology Change (TC) flag in it's BPDUs
  
  ## CST (Common Spanning Tree) [IEEE 802.1D] (default)
    * All VLANs use a common STP topology
    
  
  
  ## PVST (Per VLAN Spanning Tree) [cisco] (same as CST) 
    * Used over ISL trunks
    * Works with vlans
    * A Root Bridge is elected for every VLAN 
  ## **PVST+** [cisco] (new version of PVST)
    * Used over IEEE 802.1Q trunks
    * Works with vlans
    * A Root Bridge is elected for every VLAN 
  
  
  
  ## MISTP (Multiple Instances Spanning Tree Protocol)
  ## MSTP (Multiple Spanning Trees Protocol) [IEEE 802.1S]
    * Uses 
    
  ---
  
  # **Rapid**
  * Uses fast changing of root bridge
  * Topology change when a non-edge port transitions to the forwarding state.
  * Topology change notification is sent directly by the switch experiencing the topology change (no need to pass by the root bridge)
  * Fast convergence
  
  ## 1. RSTP (Rapid Spanning Tree Protocol) [IEEE 802.1W] 
    * Doesnt work with vlans 
    * Topology change when a non-edge port transitions to the forwarding state.
  
  
  ## 2. **RPVST** (Rapid PVST+) [cisco]
    * Works with vlans 
      * Runs a seperate instance of PVST+ for each VLAN
    * Port roles:
      * RP
      * DP
      * Disabled: Administratively shut down
      * Alternate Port [BLK]: A port on a switch that is currently discarding data frames, but could provide an alternate path to get to the root bridge (that is, an alternate to the root port) 
      * Backup Port: A port that is currently discarding data frames, although it could be an alternate path to the root bridge, and it is also acting as a redundant link to a **shared segment** (typically with Hubs; since when we conenct 2 cables between a switch & a hub, they are on the same network segment)
    * Port states:
      * Discarding: Data is not being forwarded on the port. (BPDUs are still received) (Seen on Alternate, Backup & Disabled ports)
      * Learning: The switch is learning MAC addresses available off of the port. (Seen when a port is transitioning to Forwarding)
      * Forwarding: Data is being forwarded on the port. (Seen on Root & Designated ports)
    * Link types:
      * Point-to-Point:
        * Interconnects 2 devices only
        * Full duplex (connected to 1 switch or rooter only)
      * Shared (connected to multiple switches via a hub):
        * Interconnects a switch with a shared media hub
        * Half duplex
      * Edge Port
        * Connected to a network endpoint (PC)
        * If a switch receives a BPDU from an edge point, it'll automatically change it's type
  
  
  
  # Cost table
  **Memorize this** 
  The cost of passing by a link with a
  
  
  | Speed   | Cost |
  | ------- | ---- |
  | 10Mb/s  | 100  |
  | 100Mb/s | 19   |
  | 1Gb/s   | 4    |
  | 10Gb/s  | 2    |
  
  ---
  
  # Determine ports states from a topology
  
  * Determine the root bridge (using the lowest BID)
  * Asign the cost of each link (use the STP cost table)
  * Assign the Root Ports (RP)
    * Based on the lowest:
      * Cost
      * Remote BID
      * Remote Port priority
    ```
    * Use the cost to determine which route is with the lowest:
      * Cost to go to the Root Bridge (If 2 links have the same cost, we choose the lowest )
      * Remote BID (that we send to) (the one we go to); 
      * Remote Port ID [port_priority|port_number+2] that we send to (the one we go to)
    ```
  * Assign the Designated Ports (DP)
    * On the **segments** which doesn't have a RP on them, we look for the port of that segment with the least cost to go to the Root Bridge. The one with the lowest cost is considered a DP 
    * If on a segment, the 2 paths have the same cost to go to the Root Bridge, the DP is chosen based on the lowest **BID** (Bridg ID)
  * Assign the Blocking Ports (BLK)
    * The remaining unassigned ports are blocking ports 
  
  ---
  
  # STP convergence times
  
  * Phases:
    * If a link goes down (not if it wasn't connected), we wait to receive a BPDU during a **blocking** time (we dont quickly change the root bridge, instead, we wait if he may come back)
    * After the blocking time, if we don't receive a BPDU we start the listening phase where we examine BPDUs received from an interface
    * After the listening time, we move to the learning state where the switch will send it's MAC address table **with MAC addresses seen from the concerned interface**
    * Finally, we change to the forwarding state & data packets start being sent
  
  * Timers: 
    * Blocking (Spanning tree wait time) 
      * Max age 20s (in case of CST/PVST)
      * or
      * Discarding 1~2s (in case of RPVST/RSTP)
    * Listening 15s
    * Learning 15s
    * Forwarding (as long as link is up)
  
  
  
  BPDU - Bridge Protocol Data Unit
  A type of packet exchanged in an STP topology that is used to determine which switch is the root bridge. The BPDU is sent by the Root Bridge & is used to determine if a route to the Root Bridge is existing
  
  ---
  
  ## PortFast 
  
  * Configured on ports connecting to network endpoints
  * Can be enabled globally or on a port-by-port basis (for non-trunking ports)
  * Allows a switch port to go active almost immediately when an end station is attached to the port
  * Reduces the STP wait time (Listening, Learning) on end devices ports (mode access)
  
  
  ---
  
  
  ##  BPDU guard
  
  * This option has to be configured on all ports that we dont want to receive BPDU updates from (not access mode ports)
    * Stop a broadcast storm if we accidentally plug in another switch
    * Prevent someone from changing the root bridge in the topology 
  * Should be enabled on ports with PortFast enabled
  * Causes a port to go into an error-disabled state if a BPDU is received
  * Can be enabled:
    * Globally: Will be automatically applied to all ports with PortFast enabled on them
    * On interface: Manually enable BPDU guard on an interface
  
  
  
  ## STP root guard 
  
  Guards the root bridge ports (the ports on the root connecting to the other switches) & shuts them down in case if someone wants to take control and become the root bridge. It guards the switch that is currently root from being stolen
  Apply this on the root switch ports attached with the other switches
  
  
  ---
  
  ## STP port priority 
  
  Port priority is from 0 to 240 with an increment of 16 
  Each switch port has it's own priority
  In case of 2 links or more between 2 switches port priority of **the facing port** is checked. The lower number is the highest in priority 
  If the priority is the same between the 2 ports, the port number of **the facing port** is checked. The lower number is the highest in priority 
  
  
  ---
  # Configuration
  
  * Check the STP mode | BPDU guard globally
  ```
  Switch#show spanning-tree summary
  ```
  
  * Change the STP mode 
  ```
  (conf)#spanning-tree mode (pvst|rapid-pvst)
  ```
  
  * Change the STP priority of a switch
  ```
  (conf)#spanning-tree vlan <vlan_num> priority <num>
  # or 
  (conf)#spanning-tree vlan <vlan_num> root (primary|secondary) 
  ```
  
  * Activate portfast
  ```
  (config)#spanning-tree portfast default 
  # or 
  (conf-if)#spanning-tree portfast
  # Check portfast with
  Switch#show spanning-tree interface f0/1 portfast
  ```
  
  * Deny the reception of BPDUs on an interface (BPDU guard)
  ```
  Switch(config)#spanning-tree portfast bpduguard default
  # or ...
  (config-if)#spanning-tree bpduguard enable
  ```
  
  * Root guard
  ```
  (config-range-if)# spanning-tree guard root
  ```
  
  * Change the port priority of an interface
  ```
  (config-if)#spanning-tree vlan 1 port-priority <0-240>
  ```
  
  * Change the cost of an interface
  ```
  (config-if)#spanning-tree cost <cost>
  ```
  
  * Manually change (set) the RPVST+ link type to shared or point-to-point
  ```
  Switch(config-if)#spanning-tree link-type ?
    point-to-point  Consider the interface as point-to-point
    shared          Consider the interface as shared
  ```
  
  * Set the RPVST+ link type to edge
  ```
  Switch(config-if)#switchport mode access
  Switch(config-if)#spanning-tree portfast
  ```
'''
