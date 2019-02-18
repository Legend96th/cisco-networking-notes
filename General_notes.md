  # General notes
  
  * Might want to manually set the speed (link speed) & duplex (half/full) mode on devices because auto settings might crash
  * A combination of an IP address & a port is called a socket 
  
  
  
  ---
  
  
  # Memories
  
  * Flash: IOS
  * NVRAM: Startup config
  * RAM: Running config
  
  
  
  ---
  
  # Devices
  
  * IPS: Intrusion Prevntion System
    * Prevents attacks by dropping suspecious incoming packets from entering the network
    * Sits in line with the traffic
    * It's the first device after the router connected to an unsecure connection
    * Uses a large database of well known attack header signatures
  * IDS: Intrusion Detection System
    * Receives a copy of the traffic on the network
    * Can recognize an attack using a pattern
    * Can stop an attack by sending an instruction to the router to stop receiving packets from the source of the attack
    * Sits with the end devices (as an endpoint)
    * Uses a large database of well known attack header signatures
  * Firewalls
    * Types:
      * Packet filter: Permit or deny traffic based on source/destination IP & port numbers
      * Stateful firewall: In addition to packet filtering, it can inspect sessions and recognize return traffic for a reason that was initiated from a trusted network
      * Application layer firewall: In addition to stateful, it understands the nature of an application (if it uses different protocols)
  * Wireless Access points
    * Autonomous AP: An AP that is managed individually
    * Lightweight AP: An AP that can be managed in group with a wireless LAN controller
