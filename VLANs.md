  # VLANs
  
  Virtual Local Area Network
  
  * Benefits:
    * Segmentation (based on user type)
      * Organisation
      * Tshooting
    * Reduce broadcast domains
      * Security
      * Increased performance
    * Flexibility
      * Extend the network without loosing performance
      * Dynamically affect users to VLANs
    * Improved managability
    * Physical topology independance
  * Created:
    * Statically: manually [[recommended]]
    * Dynamically: VTP
  * VLANs are created **on all switches of the same bloc**. [[recommended]]
  * Affected:
    * Statically: manually by port and won't change
    * Dynamically:
      * VMPS (VLAN Membership Policy Server): Uses a servere to attribute **MAC addresses** to VLANs
      * 802.1x [[recommended]]
  * ID range 1 - 1005 - 4094
    * Usable VLANs: 2 - 1001; 1006 - 2094
    * Reserved 1; 1002 - 1005: FDDI & Token Ring: automatically created and cannot be removed
  * By default all ports are set to the native VLAN 1 
    * Its [[recommended]] to:
      * Create an un-used VLAN
      * Affect all un-used ports to it
      * Delete it
  * VLAN IDs are selected based on these [[recommendations]]
    * Bloc
      * Don't use the same VLAN IDs in different blocs
    * Physical position
    * Logical division 
  * VLAN configurations are stored:
    * VTP client/server mode: VLAN database called flash:vlan.dat
    * VTP transparent/off(v3) mode: running-config
  
  ## Implementation
  
  1. Creation (with the managment and native VLAN)
  2. Affect ports to VLANs
  3. Setup the Trunks
  
  ---
  
  ## Notes and [[recommendations]]
  
  * Switches of the same bloc **must have the same vlans**
  * If you delete a VLAN, all the ports assigned to it will no longer be assigned to any VLAN including VLAN 1, thus making them unable to communicate with any part of the network
  * To permenantly delete **ALL** the vlans during a config restore the flash:vlan.dat must be deleted with the startup-config
  
  
  ---
  # Native VLAN
  
  * By default all ports are set to the native VLAN 1 
  * It is used to send un-tagged traffic on a trunk link
    * Switch originated traffic
    * Pass-through devices
    * Virtualized servers
  
  
  ---
  
  # Voice VLAN
  
  * A VLAN that is used to connect VoIP equipments
  * The port connecting to the switch can be
    * Single VLAN: In case the IP phone is software
    * Multi-VLAN: 
      * An access port can be used with 2 VLANs only if one of them is a voice VLAN.
      * Voice VLAN is taged using
        * CDP (NOT recomended)
        * Use DHCP to allow the phone to tag voice packets in voice vlan
    * Trunk
  * IP phones can be connected to a switch on one side & to a PC on the other, the  PC sends it's traffic to the switch passing by the IP phone. This means that the port connecting the IP phone to the switch has 2 VLANs connected to it.
  
  Configuration
  
  ```
  Switch(config-if)#switchport voice vlan <vlan-num|dot1p>
  ```
  
  ---
  
  # Trunks
  
  * Transports multiple vlans on a single link
  * Uses a trunk tag (encapsulation) to diffrentiate between VLANs
  * Trunk encapsulation (tag) types
    * Dot1Q (IEEE) (4 bytes tag) added between the source MAC address and the ethertype fields
      1. TPID (Tag Protocol Identifier) (2 bytes) set to 0x8100
      2. TCI (Tag Control Information) (2 bytes)
          * PCP (Priority Code Point) (3 bits): Refers to the CoS field
          * DEI (Drop Eligable Indicator) (1 bit): Used with PCP to distingish frames eligable to be dropped in the case of a congestion   
          * VLAN ID (12 bits): Used to carry the vlan ID
            * Reserved values:
              * **0x000**: Frame does not carry a VLAN ID; the 802.1Q tag specifies only a priority (in PCP and DEI fields) and is referred to as a **priority tag**
              * 0xFFF: Reserved; it must not be configured or transmitted      
    * ISL (Cisco) (30 bytes tag)
  * VLAN tagged packets should only circulate in interconnecting devices
  * By default all vlans are allowed on a trunk
  * No trunk will be up if there's no vlan created with at least one port assigned to it
  
  
  ## Notes and [[recommendations]]
  * Native VLAN is **NOT** tagged on trunk links! 
    * Make the switch tag it [[recommended]]
  * On routers connected to a trunk interface, add `native` to the native vlan subinterface's encapsulation command
  
  ## Implementation
  1. Encapsulation
    * 802.1q [[recommended]]
      * Must be configured on some platforms (MLS)
    * ISL
  2. Change switchport to trunk mode
  3. Match native VLAN (!= management vlan)
  4. Setup the allowed VLANs
  5. Stop the DTP negotiation
  6. Add native vlan tagging `# dot1q tag native vlan`; not availiable on all platforms
  
  
  
  
  
  
  ---
  
  # VTP
  Vlan Trunking Protocol
  
  * Use one switch (server) to configure VLANs, others take the config from it
  * **Works only on trunk interfaces**
    * No trunk => no VTP [[important]]
  * All switches must have the same 
    * VTP domain name
    * Password (optional)
  * VTP pruning: VLANs not to be advertised
  * CRN (Configuration Revision Number):
    * A counter that increments every time a configuration is applied
    * Increments only when **mode is server**
    * Changes on **clients and servers only** (by vtp synch)
    * Doesn't change on transparent switches
    * The switch with the higher CRN is the master switch, others obey the same config with this switch
    * To reset the CRN
      * Change the switch to a transparent switch then change it to the desired mode 
      * Delete flash:vlan.dat [optional]
  * Messages are sent when:
    * A change in the vlan database is made (CRN increments)
    * Receiving an update with a lower CRN (VTP synchronisation)
      * This means that a Switch is not syched and it's sending false updates => update it
    * Lightweight periodic messages (VTP domain name, CRN...etc ) are sent every 5 minutes
  * Modes:
    * Server (default)
      * Can be used to create/delete/update VLANs
      * Can originate VTP advertisements 
      * If a VTP advertisement is received:
        * Higher CRN
          * Updates it's VLAN database based on received advertisement
          * Forwards received VTP messages
          * Applies the received updates
        * Lower CRN
          * Synchronise the sender
        * Equal CRN
          * Drop
    * Client
      * Cannot be used to create/delete/modify VLANs
      * Updates it's VLAN database based on received advertisements
      * Forwards received VTP messages
      * Can originate VTP advertisements
        * If a VTP message is received with a lower CRN, the sender will be synched
    * Transparent: passes the VLANs but doesn't apply them
      * Can be used to create/delete/modify VLANs
      * Does not update it's VLAN database based on received advertisements
      * Forwards received VTP messages
      * Does not originate VTP advertisements
      * [[**WARNING**]] If using vlans, we must creat at least one vlan (to enable encapsulation) on the transparent switch to allow vlans to be tagged & pass 
    * Off (v3 only)
  * Versions:
    * V1:
      * If a switch is set to transparent mode, it'll take a look at the VTP message that it received before forwarding it. Checking the the domain name & version matched.
      * If a VTP message doesn't have the same domain & version the transparent mode switch won't forward it
    * V2: 
      * Added support for token ring LAN switching & token ring VLANs
      * If a new switch is plugged in the network, it'll automatically pick up the domain name from a received VTP message & configure itself with it
    * V3:
      * Allowed additional VLAN numbers to be advertised via VTP
      * Support for MST (an STP version)
  
  ## Note:
  * Disable and never use VTP [[recommended]]
  * If you plug in a new switch to the network, make sure it has lower CRN otherwise it'll ruin the installation 
    * To avoid having problems, reset the CRN 
  * VTP password and domain are stored in flash:vlan.dat [[key topic]]
  * `service pass-ecrypt` only encrypts passwords stored in running-config => it won't encrypt the VTP password
  * If no domain name is configured, the first domain that is received will be set to the VTP domain [[warning]]
  * A switch ignores and discards an update if:
    * VTP domain is different
    * Password is different
    * CRN is **lower or *equal***
      * If same domain & password
        * If CRN 
          * Lower: sync 
          * Equal: drop
  
     
  
  
  # Config process
  * Create vlans on server
  * Set interfaces (between swithes to trunk)
  * Set vtp mode (client/server/transparent)
  * Set vtp domain
  * Set vtp password
  
  
  
  ---
  
  # DTP
  Dynamic Trunking Protocol (CISCO)
  * Allows a switch port to dynamically negotiate the formation of a trunk between 2 switches
    * This negotiation should be stopped
  * Only works between **cisco switches** (not routers nor non cisco products) [[keytopic]]
  * Used to stop VLANs from passing on access ports
  * Used to secure the link against VTP attacks
  * Types (states) of ports in a switch
    * Access: transports 1 (or 2) VLAN 
    * Trunk: transports more than 1 VLAN
    * Dynamic desirable (default on all MLS except 3550): Initiates the negotiation of a trunk
    * Dynamic auto (default on all 2xxx platform + 3550): Passively waits for the remote switch to initiate the negotiation of a trunk
  
  
  |SW1\\SW2 | Access | Trunk | Dyncamic desirable | Dynamic auto|
  |-|-|-|-|-|
  |Access | Access | X | Access | Access | Access|
  |Trunk | X | Trunk | Trunk | Trunk|
  |Dyncamic desirable | Access | Trunk | Trunk | Trunk|
  |Dyncamic auto | Access | Trunk | Trunk | Access|
   
  
  ## Allowed VLANs on trunk
  
  Used to allow only some VLANs to pass
  
  * Allow a few
  * Allow all except a few
  * Allow all
  * Deny all
  
  
  * Stop DTP negotiation
    * `Switch(config-if)#switchport nonegotiate`
  * Check (type of a port | negotiation status)
    * `Switch#show int f 0/1 switchport `
  * Show the trunk ports
    * `Switch#show interfaces trunk`
  * Change the encapsulation type
    * `Switch(config-if)#switchport trunk encapsulation <type>`
  * Change the native VLAN
    * `Switch(config-if)#switchport trunk native vlan <VLAN-ID>`
  
  * Change VLANs passing on a trunk interface (VLAN pruning)
  ```
  Switch(config-if)#switchport trunk allowed vlan ?
    WORD    VLAN IDs of the allowed VLANs when this port is in trunking mode
    add     add VLANs to the current list
    all     all VLANs
    except  all VLANs except the following
    none    no VLANs
    remove  remove VLANs from the current list
  ```
  
  * Change switchport mode
  ```
  Switch(config-if)#switchport mode ?
    access   Set trunking mode to ACCESS unconditionally
    dynamic  Set trunking mode to dynamically negotiate access or trunk mode
    trunk    Set trunking mode to TRUNK unconditionally
  ```
'''
