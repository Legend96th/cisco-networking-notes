createdAt: "2018-09-09T09:06:19.802Z"
updatedAt: "2019-02-06T19:53:17.194Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "L3 switch with inter-vlan"
content: '''
  # L3 switch with inter-vlan
  
  * Layer 3
  * L3 like router
  * Fast routing
  * IP cef (Cisco Expres Forwarding)
  * Device references 
    * 2000 < ref < 2999: L2
    * 3000 < ref < 3999: L3
    * 4000 < ref < 4999: Core switch
  
  ---
  
  ## SVI
  Switch Virtual Interface
  
  * A virtual interface created on switched
  * Several ports can be assigned to the same SVI
  * How to set it up:
    * Create SVIs on a MLS each SVI corresponding to a VLAN
    * Activate `ip routing` on the switch
  
  
  ## Routed Port
  
  * A routed port is a port that doesn't act like a switchport but instead it acts like a router port (we can assign an IP address to it & activate a routing protocol from it)
  * A routed port is a port that is connected to a router
  * It doesn't belong to any SVI
  * An IP address is assigned to this port
  * How to set it up:
    * Configure the interface with an IP address
    * Connect it to a router
  
  ---
  
  # SVI configuration
  
  ```
  Switch(config)#int vlan <vlan_1_num>
  Switch(config-if)#ip add <ip_add_1> <mask1>
  Switch(config-if)#int vlan <vlan_2_num>
  Switch(config-if)#ip add <ip_add_2> <mask2>
  Switch(config-if)#exit
  Switch(config)#ip routing
  ```
  
  # Routed port configuration 
  
  ```
  Switch(config)#int g0/1
  Switch(config-if)#no switchport 
  Switch(config-if)#ip add <ip_add_> <mask>
  ```
  
  
  
  ## Activating L3 on a MLS
  
  ```
  Switch(config)#ip routing
  ```
  
  
'''
tags: []
isStarred: false
isTrashed: false
