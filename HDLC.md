  # HDLC
  Higher level data link control protocol
  
  The default L2 encapsulation found on cisco serial interfaces
  
  * Serial only
  * Default on serial links
  * There's an old IEEE version but now it's cisco proprietary
  * Alternative to PPP
  * Doesnt support:
    * Authentication
    * Compression
    * Error detection
  * IEEE HDLC frame content
    * CRC
    * Packet
    * Control
    * Addressing (8 Bits all set to 1)
    * Flag (preamble)
  * Cisco HDLC frame content
    * CRC
    * Packet
    * Type
    * Control
    * Addressing (8 Bits all set to 1)
    * Flag (preamble)
  
  {CLIENT}(router/modem) == [DSLAM] == (BRAS){ISP}
  
  * Connection phases:
    * Phase 1 (hand-shaking):
      * LCP (Link Control Protocol)
        * Hello exchange
          * => hello
          * <= hello
        * link operation negotiation
          * => authentication 
    * Phase 2 
      * => Supported IP version
      * <= ok
      * => Request IP address
      * <= ok *gives an IP address*
    * Phase 3
      * Data is sent
'''
