  # ACL
  Access Control List
  
  * Packet **identification** mechanism
  * Can identify packets on the basis of L2, L3 and L4 header [\\* => Entire match required]
    * L3:
      * \\*TOS
      * \\*Protocol number
      * Source IP address
      * Destination IP address
    * L4:
      * \\*Source port
      * \\*Destination port
      * \\*Flags
  * Can contain multiple ACEs (Access Control Entries)
    * Each ACE is assigned a sequence number
    * The entries are processed in sequential order (top-down) until a match is found
    * Each Entry can deny or permit something to be matched with that ACL
    * **There's an implicit "DENY ANY" statement at the bottom of an ACL**
  * Can be applied 
    * Inbound or outbound of an interface to filter traffic
      * Only 1 ACL per direction is allowed on the same interface
  * Can filter traffic
  * Can match traffic
  * ACL Types
    * Numbered ACLs:
      * Individual statements **cannot be edited or deleted** [[warning]]
      * * Configuration starts with `access-list`
      * Types:
        * Standard: Can filter traffic based on source IP address
          * Numbered (1-99) (1300-1999)
          * Based on L3
            * Source IP
          * Should be applied closest to the destination
        * Extended: Can filter traffic based on source and destination IP address
          * Numbered (100-199) (2000-2699)
          * Based on L3 & L4
            * Source and destination IP
            * Source and destination TCP/UDP port
            * TCP control flags
          * Capable of transport header **inspection**
          * Should be placed closest to the source
    * Named ACL
      * Individual statements can be edited
      * Configuration starts with `ip access-list <standard|extended>`
      * Types:
        * Standard named
        * Extended named
  * ACL Applications 
    * IP Acess group 
      * Interface-level security feature (basic firewalling)
      * Directional (must specify traffic direction: in or out)
      * References ACLs for classification/identification
        * **Only one ACL** can be used in a direction
    * NAT
      * Traffic matched by the ACL will be NATed
      
  
  # Notes and [[recommendations]]
  * If you delete an ACE from a numbered ACL in the global config mode, ALL the ACL's ACEs will be deleted!! [[WARNING]]
    * Instead you need to enter the ACL edit mode and then edit the ACE based on it's sequence number
  * Specefic policies (entries) are placed near the top and general ones near the bottom 
  * Sequence numbers can be added to insert a policy at a specified position to be applied before or after other policies
  * If TCP or UDP are specified, the exact port number or a range of ports can be selected
  * TCP traffic can be controlled to check if a session is opened for this traffic using the `established` keyword in the end of the ACE 
    * This is verified using the ACK bit; It should be set to 1 on all packets exchanged after a 3-Way handshake
    * Can be fooled by the attacker falsely setting the bit to 1 
  
  ---
  
  # IPv6 ACLs
  * Same as IPv4 ACLs
  * Standard IPv6 ACLs can filter traffic based on source and destination IP address
  * Only named ACLs
  * IPv6 NDP (Neighbor Discovery Protocol) messages are implicitly allowed to pass ACLs **but router advertisement messages are NOT allowed to pass** so don't forget to allow them
    
  
'''
