  # NAT
  Network Address Translation
  
  * Can translate (map) a private IP address to a publicly routable IP address, selected from a pool of publicly routable IP addresses or by using a single address
  * Provides security
    * Keeps the internal topology invisible to extrenals
    * Hides private addresses from the outsiders
    * Only allows packets that have a correspondance in the NAT transelation table
  * Helps reduce public IP address consumption
  * NAT types
    * Static NAT
      * One to one translation
      * Needs a public address for every local one
      * Assigned manually by the network administrator
      * Usually used for servers
    * Dynamic NAT
      * Translate multiple inside local addresses to multiple outside global addresses from a specified pool
      * Needs one public address for every local address
      * Assigned automatically by the router
      * Usually used for hosts with dynamic addresses 
      * Translations lease timers
        * TCP: 24 hours
    * NAT overloading (PAT - Port Address Translation)
      * Map multiple private (inside local) IP address to a single public (inside global) IP address
      * Needs one public address for all local addresses
      * Router uses port numbers to identify which host's address is translated
      * Could have **65000 hosts** on a single public IP address 
      * When a packet is exiting, the source port number is changed only if it's already in use by another traffic flow:
        * If the source port number isn't already used by another NATted traffic, it'll be used without being changed
        * If it's already in use by another traffic, the router generates another random un-used port to be assigned to this traffic
  
  
'''
