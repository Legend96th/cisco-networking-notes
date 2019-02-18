  # Exam notes 
  
  ***_NO TABS ALLOWED_***
  
  * A hub has 1 colission domain & 1 broadcast
  * In a switch there is 1 broadcast & as many collision domains as the devices number 
  * OSI layers size
    * Transport (4): 1480 byte
      * CRC: 2 byte
    * Network (3): 1500 byte
    * Data-Link (2): 1518 byte
  * IPv4 address: 32 bit or 4 bytes
  * IPv4 classes:
    * A 1 - 126
      * Mask 255.0.0.0
      * CIDR 8
    * B 128 - 191
      * Mask 255.255.0.0
      * CIDR 16
    * C 192 - 223
      * Mask 255.255.255.0
      * CIDR 24
    * D 224 - 239
      * Called multicast address 
    * E 240 - 255
      * Reserved for developpers
  * Addresses 0.x.x.x & 127.0.0.1 are reserved: 
    * 0 is used for default route
    * 127 is the loopback
  * In class B, addresses from 169.254.0.0 to 169.254.255.255 (APIPA) are **NOT** routable even in a LAN!
  * IPv4 header: 12 fields
  * IPv6 address: 128 bit
  * IPv6 header: 8 fields
  * Mac address: 48bit
  * Don't forget to save the work (not with "wr")
  * APIPA: an address asigned automatically in case we can't get one by DHCP
    * 169.254.x.x 
  * Load balancing: 50%/50%
  * Load sharing: x/y; x+y = 100%
  * Classless routing: 
    * Works only with network classes (A, B..etc), no subnetting
    * Must use the same class with all the networks
  * Classfull routing: 
    * Can work with both classes & subnetting
    * Can combine different classes
  * RIPv2 multicast address: 224.0.0.9
  * OSPF area 0 is called: backbone area 
  * Wildcard-MASK = 255.255.255.255 - MASK (substract field by field)
  * End devices only accept 1528 byte packets
  * VLAN tagged packets aren't accepted by end devices because they are 1522 byte
  * VLAN Taghing only happens in switches
  * We can stack only up to 8 switches; more than 8 can't be
  * Mac & ARP tables gets flushed each 5 minutes
  * Memory types in devices are:
    * Flash
      * IOS
      * vlan.dat
    * VRAM
      * Startup config
    * RAM
      * Running config
  * ACL difference:
    * Standard: Network filter only
    * Extended: 
      * Network
      *  Source & destination network or address, port & protocol
  * In ACL make sure to match the exact: port / protocol combination; make sure to check the correct port of server or client   
  * If you use an ACL with networks, must use wildcard mask
  * Wildcard mask uses
    * OSPF
    * ACL
  * Serial connection don't use MAC addresses
  
  
  
'''
