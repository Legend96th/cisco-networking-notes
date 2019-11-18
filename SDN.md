content: '''
  # SDN
  Software Defined Networking
  *Network Programability*
  
  
  A technology that allows network device configurations to be controlled through software applications
  
  * A packet flowing through the network is going  through one of 3 architechtural planes of operation on the network
    * Data plane: Responsible for anything that has to do with forwarding packets
      * Encapsulation/Decapsulation
      * Determining the port out of which a frame should exit a switch based on the destinations MAC address
      * Checking the IP routing table on a router to determine how to forward a packet
      * ACL dropping packets
      * Encryption
      * ...anything that has to do with touching the packet itself
    * Control plane: Controlling the Data plane by
      * Populating data in the MAC address table or in the routing table and creating them
      * Running the routing protocols (routing protocols live in the control plane)
      * Running the STP protocol
    * Management plane: Deals with administrative access to get into the router or switch
      * Telnet
      * SSH
      * SNMP
  * Terminology:
    * Distributed Control Plane: A network architecture where control plane functions (eg: routing protocols) reside in the network devices
    * API (Application Programming Interface): Allows applications on one device to communicate with an application on another device
    * **SBI (Southbound interface)**: An API used for communication between an SDN controller and a network device
    * Centralized control plane: A network architecture where the control plane functions reside in a centralized SDN controller
    * ACI (Cisco Applicaton Centric Infrastructure): Cisco's SDN architecture
    * APIC (Application Policy Infrastructure Controller): A component in Cisco ACI that acts as an SDN controller, which commonly uses **OpFlex** as its API
    * OpFlex: A vendor interoperable  SBI
    * **NBI (Northbound interface)**: An API used for communication between the user control **applications** and the SDN controller
    * REST API (Representational State Transfer API): An API, commonly written as RESTful API that uses HTTP messages (GET,PUT,POST...) to  send information from an application to an SDN controller 
    * JSON (JS Object Notation)
