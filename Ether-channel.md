  # Ether-channel
  
  Groups multiple ports to form one logical port that sums the physical ones thus getting a sum of of the speed
  
  * Types of protocols
    * LACP (Link Aggregation Control Protocol) (IEEE 802.3AD)
    * PAgP (Port Aggregation Protocol) (CISCO)
  * Algorithms:
  *The number of the last **n** bits to look at is the **n** solving (2^n = number_of_links)*
    * Destination IP address
    * Destination MAC address
    * Source IP address
    * Source MAC address
    * Source & destination IP address 
      * Perform a XOR between the last 'n' bits of source & destination IP address
    * Source & destination MAC address
      * Perform a XOR between the last 'n' bits of source & destination IP address
  * Things to check on each port of both end
    * Speed
    * Duplex
    * VLAN assignment
  * EtherChannel guard
    * A feature that can detect mismatched channel parameters between switches, generate an error message, &nd place a poirt into an error disabled state
  * Types of etherchannels
    * Layer 2 EtherChannel
      * A connection make up of a logical grouping of ports into a virtual interface that is configurable as a Layer 2 interface
      * Sending the same VLANs on trunks
    * Layer 3 EtherChannel
      * A connection make up of a logical grouping of ports into a virtual interface that is configurable as a **routed interface**
  
  
  ---
  
  ## Etherchannel formation
  
  
  | LACP | On | Passive | Active |
  |-|-|-|-|
  | On | Y | X | X |
  | Passive | X | X | Y |
  | Active | X | Y | Y |
  
  
  | PAGP | On | Auto | Desirable |
  |-|-|-|-|
  | On | Y | X | X |
  | Auto | X | X | Y |
  | Desirable | X | Y | Y |
  
  *Types:*
  * On: This interface is an etherchannel (not initiating but accepting if received a request or if the facing interface is also set to on)
  * Auto: Don't initiate a communication but accept if received
  * Desirable: Initiate & accept a received communication
  
  ---
  
  ## Activation
  
  ### L2 EtherChannel
  * Select the interfaces (**Maximum of 8 interfaces to work**)
  * Select the protocol
  * Select the group & mode
  
  ```
  Switch(config-if-range)#speed auto 
  Switch(config-if-range)#duplex auto
  Switch(config-if-range)#mdix auto 
  Switch(config-if-range)#channel-protocol (lacp|pagp)
  Switch(config-if-range)#channel-group <grp_num> mode ((active|passive)||(auto|desirable))
  Switch(config-if-range)#exit
  Switch(config)#interface port-channel <grp_num> 
  ```
  
  ### L3 EtherChannel
  **check the video again**
  ```
  Switch(config-if-range)#speed auto 
  Switch(config-if-range)#duplex auto
  Switch(config-if-range)#mdix auto 
  Switch(config-if-range)#no switchport 
  Switch(config-if-range)#channel-protocol (lacp|pagp)
  Switch(config-if-range)#channel-group <grp_num> mode on
  Switch(config-if-range)#exit
  Switch(config)#interface port-channel <grp_num> 
  Switch(config-if)#no switchport
  Switch(config-if)#ip address <ip> <mask>
  .... check the video again
  ```
  
  * Show informations about an etherchannel
  ```
  Switch#show interfaces port-channel 1
  
  Switch#show etherchannel summary
  
  Switch#show etherchannel port-channel
  ```
  
  * **Enable** EtherChannel guard
  *enabled by default & can be checked with `Switch#show spanning-tree summary`*
  ```
  Switch(config)#spanning-tree etherchannel guard misconfig
  ```
  
  * Check the used load balancing algorithm
  ```
  Switch#show etherchannel load-balance
  ```
  
  * Change the used load balancing algorithm
  ```
  Switch(config)#port-channel load-balance ?
    dst-ip       Dst IP Addr
    dst-mac      Dst Mac Addr
    src-dst-ip   Src XOR Dst IP Addr
    src-dst-mac  Src XOR Dst Mac Addr
    src-ip       Src IP Addr
    src-mac      Src Mac Addr
  ```
