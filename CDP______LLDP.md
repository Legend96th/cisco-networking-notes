  # CDP // LLDP
  
  # CDP
  Cisco Discovery Protocol 
  
  * Cisco propretary
  * Allows to discover nearby (connected) devices
  * Provides informations about adjacent CDP-Speaking devices (Interface, IP address, OS version, device type...etc)
  * Layer 2 (not routable; if a device receives a CDP message, it cannot forward it to another, it can only keep it for itself)
  * Sends hello messages to multicast MAC address of **01:00:0C:CC:CC:CC**
  * Timers:
    * Hello: 60s
    * Holdtime: 180s
  
  
  # LLDP
  Link Layer Discovery Protocol
  
  * IEEE 802.1ab
  * Layer 2
  * Provides informations about adjacent LLDP-Speaking devices
  * Limited to work only on 802.1* media types (Ethernet) not on WAN links
  * Sends to multicast MAC address with an OUI of 01:80:C2:00:00:0(0|E|3)
  * Timers:
    * Hello: 50s
    * Holdtime: 150s
  * Uses MED (Media Endpoint Discovery) as an enhancement for VoIP applications
  
  ---
  # Table of comparison 
  
  | \\ |Origin|OSI Layer|Supported media|Multicast address|Hello Timer|Holdtime|
  |=|=|=|=|=|=|=|
  |CDP|CISCO|2|Any|01:00:0C:CC:CC:CC|60s|180s|
  |LLDP|IEEE|2|802.1$*$|01:80:C2:00:00:0(0\\|E\\|3)|15s|150s|
  
  *We can have both CDP and LLDP running on the same interface*
  
  * CDP/LLDP must be stopped: 
    * Can be used to perform L2 tracing (mac address, port numbers)
    * Can be used to inject routes in routing tables (ODR - On Demand Routing)
  
'''
