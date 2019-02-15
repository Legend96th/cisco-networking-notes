createdAt: "2018-09-04T09:55:28.764Z"
updatedAt: "2019-01-25T21:48:27.489Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "VLANs"
content: '''
  # VLANs
  
  Virtual Local Area Network
  
  * VLAN range 1 - 1005
  * By default all NICs are connected to the native VLAN 1
  * Custom VLANs start from 2 - 1001
  * VLANs 1002 - 1005 are reserved for Token Ring & FDDI VLANs
  * VLANs 1, 1002-1005 are automatically created and cannot be removed
  * Configurations are stored withing a VLAN database called vlan.dat
  * The vlan.dat file is located in the flash memory of the switch
  * To permenantly delete **ALL** the vlans (restore config) the vlan.dat must be deleted using `Switch#delete flash:vlan.dat`
  * Types of ports in a switch
    * Access: transports 1 VLAN
    * Trunk: transports more than 1 VLAN
      * Trunk tag (the one that diffrentiates VLANs) - 4 byte
      * VLAN tagged packets only circulate in interconnecting devices
    * Dynamic desirable: Initiates the negotiation of a trunk
    * Dynamic auto: Passively waits for the remote switch to initiate the negotiation of a trunk
  * Check type of a port
    * `Switch#show int f 0/1 switchport `
  * Check the trunk ports
    * `Switch#show interfaces trunk`
  * Encapsulation (tag) types
    * Dot1Q (IEEE) (4 bytes tag) 
      1. Tag protocol identifier (2 bytes)
      2. Tag control identifier (2 bytes)
      * Composed:
        * VLAN ID (12 bits)
        * QoS marking (3 bits)
    * ISL (Cisco) (30 bytes tag)
  * Change the encapsulation type
    * `Switch(config-if)#switchport trunk encapsulation <type>`
  * Native VLAN is **NOT** tagged!
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
  
  ## Benefits of VLANs
  
  * Increased performance
  * Improved managability
  * Physical topology independance
  * Increased security options
  
  ---
  
  ## Notes
  
  * VLANs stop broadcast messages from crossing VLANs
  * If you delete a VLAN, all the ports assigned to it will no longer be assigned to any VLAN including VLAN 1, thus making them unable to communicate with any part of the network
  
  ---
  
  # Voice VLAN
  
  * A VLAN that is used to connect VoIP equipments
  * IP phones can be connected to a switch on one side & to a PC on the other, the  PC sends it's traffic to the switch passing by the IP phone. This means that the port connecting the IP phone to the switch has 2 VLANs connected to it.
  * The port connecting to the switch can be
    * Single VLAN: In case the IP phone is software
    * Multi-VLAN: 
      * An access port can be used with 2 VLANs only if one of them is a voice VLAN.
      * CDP must be activated to enable this feature
    * Trunk
  
  Configuration
  
  ```
  Switch(config-if)#switchport voice vlan <vlan-num|dot1p>
  ```
    
  
  ---
  
  # VTP
  Vlan Trunking Protocol
  
  * Use one switch to configure VLANs, others take the config from it
  * All switches must have the same 
    * VTP domain name
    * Password (optional but it's a best practice to use)
  * If we don't want some VLANs to be advertised to some switches we must enable VTP pruning
  * Messages:
    * When a change is made a message is sent to other switches immediatly
    * Lightweight periodic messages (VTP domain name, CRN...etc ) are sent every 5 minutes
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
  * Modes:
    * Server (default)
      * Can be used to create/delete/modify VLANs
      * Updates it's VLAN database based on received advertisements
      * Forwards received VTP messages
      * Can originate VTP advertisements
    * Client
      * Cannot be used to create/delete/modify VLANs
      * Updates it's VLAN database based on received advertisements
      * Forwards rceived VTP messages
      * Can originate VTP advertisements
    * Transparent: passes the VLANs but doesn't apply them
      * Can be used to create/delete/modify VLANs
      * Does not update it's VLAN database based on received advertisements
      * Forwards received VTP messages
      * Does not originate VTP advertisements
      * **WARNING**: If using vlans, we must creat at least one vlan (to enable encapsulation) on the transparent switch to allow vlans to be tagged & pass 
  
  **Note:**
  If you plug in a new switch to the network, make sure it has lower CRN otherwise it'll ruin the installation 
    * To avoid having problems, reset the CRN 
  
     
  ### CRN
  Configuration Revision Number
  * A counter that increments every time a configuration is applied to the switch
  * The switch with the higher CRN is the master switch, others obey the same config with this switch
  * To reset the CRN
    * Change the switch to a transparent switch then change it to the desired mode 
    * Delete VLAN.dat [optional]
  
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
  * Used to stop VLANs from passing
  * Used to secure the link against VTP attacks 
    * Dynamic desirable: follows the rules on the other end
    * Dynamic auto: (default)
    * Access
    * Trunk
  
  
  |SW1\\SW2 | Access | Trunk | Dyncamic desirable | Dynamic auto|
  |-|-|-|-|-|
  |Access | Access | X | Access | Access | Access|
  |Trunk | X | Trunk | Trunk | Trunk|
  |Dyncamic desirable | Access | Trunk | Trunk | Trunk|
  |Dyncamic auto | Access | Trunk | Trunk | Access|
   
  * Change switchport mode
  ```
  Switch(config-if)#switchport mode ?
    access   Set trunking mode to ACCESS unconditionally
    dynamic  Set trunking mode to dynamically negotiate access or trunk mode
    trunk    Set trunking mode to TRUNK unconditionally
  ```
  
  ## Allowed DTP
  
  Used to allow only some VLANs to pass
  
  * Allow a few
  * Allow all except a few
  * Allow all
  * Deny all
  
  
'''
tags: []
isStarred: false
isTrashed: false
