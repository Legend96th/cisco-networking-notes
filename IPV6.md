   # IPV6  
  
  *APIPA (self assigned address)*
  
  * Size: 128 bit
  * Header: 8 fieldss
  * Hexadecimal
  * Format: 8 fields 4 positions each
    * 1111:2222:3333:4444:5555:6666:7777:8888
  * Leading zeros '0' can be removed
  * A field of zeros '0' can be replaced with one 0
  * Adjacant fields of zeros '0' can be replaced with 'x:x::x:x' 'x:x::'
    * This rule can only be used once 'x::x:0:0:x'
  * No broadcast
  * No fragmentation: MTU discovery is performed for each session
  
  ---
  
  # Address types
  
  ## Unicast
  * Loopback ::1
  * Unspecified :: (all zeros)
    * Used for a client's source address when sending a:
      * Neighbor Solicitation message
      * Router Solicitation message
  * Link local
    * Can only communicate on one network segment
    * Similar to IPv5 APIPA address (169.254.0.0/16); not routable
    * Can be automatically or statically assigned
    * Starting with 'FE80:x:x:x/10'
    * Composition:
      * 1111 11110 10 (10 bits)
      * 54 zeros (54 bits)
      * Interface ID (64 bits)
  * Unique local
    * Addressing starts with FC00::/7
    * Cannot be routed over public internet; similar to private IPv4 addresses (can be routed internally)
    * Composition:
      * 1111 110 (7 bits)
      * L (1 bit)
        * If set to 1 => address is locally assigned (so in practise the addresses start with FD00::/7)
      * Global ID (40 bits)
        * Used to create a global unique prefix
      * Subnet ID (16 bits)
        * Used to identify a subnet within a site
      * Interface ID (64 bits)
  * Site local
    * Private IPs
    * Starting under '2001'
  * Global local (Abderrazak)
    * public IPs
    * starting '2001' or higher
  * Global Unicast (Kevin Wallace)
    * Global: Routable on public internet
    * Unicast: 1 to 1 communication flow
    * Addressing starts with 2000::/3
    * Addressing assigned by the IANA
    * Composition:
      * 001 (3 bits)
      * Global Routing Prefix (45 bits)
      * Subnet ID (16 bits)
      * Interface ID (64 bits)
  
  ## Multicast 
  * Abderrazak
    * eg: OSPFv3, EIGRPv6, RIPng
    * Multicast starting with 'ff08'
  * Kevin Wallace
    * Has an FF as the first 2 hexadecimal digits
    * Composition:
      * 1111 1111 (8 bits)
      * Flags (4 bits)
        * 0RPT
          * 0: Reserved bit set to 0
          * R: [if (R==1) then P = T = 1]: Indicates that there is a Rendez-vous point (RP) address embedded in the multicast address
      * Scope (4 bits) 16 possibilities
        * 0: Reserved
        * 1: Interface-Local Scope
        * 2: Link-Local Scope (A subnet)
        * 3: Reserved
        * 4: Admin-Local Scope
        * 5: Site-Local Scope
        * 6: Unassigned
        * 7: Unassigned
        * 8: Organization-Local Scope
        * 9: Unassigned
        * A: Unassigned
        * B: Unassigned
        * C: Unassigned
        * D: Unassigned
        * E: Global Scope
        * F: Reserved
      * Group ID (112 bits)
    * Solicited-Node Multicast Address
      * Used instead of IPv4 ARP to discover neighbors's IPv6 & MAC address
      * Used for Duplicate Address Detection (DAD)
      * Composition:
        * FF02::1:FF (104 bits)
        * Last 24 bits in IPv6 address (24 bits)
  ## Anycast
  * One to nearest
  * If 2 devices have the same IPv6 address, the nearest one is chosen
  
  **Note**
  * Rendez-vous point (RP): 
    * A router which forwards multicast traffic to routers asking to receive traffic
    * Used in PIM sparese mode
  * PIM (Protocol Independent Multicast): A multicast routing protocol, which operates in sparse mode (efficient, uses a router as a RP) or dense mode
  
  ---
  
  # Address attribution methods in IPv6
  
  ## 1. Full Static
  ## 2. Half static - 64-bit Extended Unique Identifier (EUI-64) (Modified EUI-64 address)
    * Set up the network part manually & the equipement completes the address by spliting the MAC address in half & inserting "FFFE"
    * Method of work
      1. Split the 48 bit MAC address in the middle
      2. Insert "FFFE" in the middle of the splitted MAC address
      3. Change from '.' to ':'
      4. Convert the first 8 bits to binary 
      5. Flip the 7th bit (U/L bit) (if 1 then 0; if 0 then 1)
      6. Convert the first 8 bits back to hexadecimal
      7. Insert "FE80::" at the start to create a Link-Local IPv6 address
  ## 3. Auto configuration:
  ### *3.1 Statefull auto configuration*
    The address is assigned using an IPv6 DHCP server
    1. The client sends a solicite multicast message to FF02::1:2 (DHCP servers multicast address)
    2. The server responds to the client with an advertise (unicast)
    3. The clients then sends a request
    4. Finnaly the server sends a reply containing the IPv6 address that the client can use
  
  ### *3.2 Stateless auto configuration:* 
    Takes the network address (global address & local subnet information) from the router & add EUI-64 host address
    1. The client sends a node solicitation multicast packet to "FF02::1:FFxx:xxxx"
    2. If there is no response it means that no client has this address 
    3. The client then sends a router solicitation multicast to "FF02::2" (instead of waiting for a periodic router advertisment)
    4. The router then sends a router advertisment to the all nodes multicast address "FF02::1"
    5. The client then uses the router advertisment to construct its EUI-64 globally unique IPv6 address
  
  
  **Note**
  Routers periodically sends a router advertisment 
  
  * SAS
  * SLACK
  
  
  ---
  ----
  # Commands
  
  ## Activate IPv6 routing
  `ipv6 unicast-routing`
  
  ## Configure the interface with a specefic IPv6 address
  * Get an automatic Link Local address `Router(config-if)#ipv6 enable`
  * Setup a manual address `Router(config-if)#ipv6 address 2001:24:0:1::34f/64`
  
  ## OSPF IPv6 routing
  
  ```
  en
  conf t
  ipv6 unicast-routing
  ipv6 router ospf 1
  router-id 1.1.1.1
  interface gigabitEthernet 0/0
  ipv6 address 1::1/64
  ipv6 ospf 1 area 0
  no shutdown
  exit
  ```
  
  configure router
  * (conf) ipv6 router opsf 1
    * (conf) ipv6 router opsf 1
  * (config-rtr) router-id <id(ipv4 style id)>
    * (config-rtr) router-id 0.1.2.3
  
  
  configure interface
  * (conf-inf) ipv6 ospf <process_id> area <area_num>   
    * (conf-inf) ipv6 ospf 1 area 0 
  
'''
