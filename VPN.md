createdAt: "2018-09-12T10:28:43.470Z"
updatedAt: "2018-09-12T20:49:04.057Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "VPN"
content: '''
  # VPN
  Virtual Private Network
  
  Types:
  * Site to site VPN
    * Connect to a LAN from the outside
    * External IP address must be fixed
  * VPN client
    * Needs a special application
  * SSL VPN
    * Uses a browser directly
  
  ## Pros of VPN
  * Confidentiality
      (Encryption)
    * Symetric
      Uses a private key
      Types:
      * DES (64bit)
      * 3DES
      * AES (192bit)
    * Asymetric
      Uses a private & a public key
      Types:
      * RSA
        * (web auth): Client sends a request, server sends it's public key, client sends his private key encrypted with the public key to open the account
      * Diffe-Hallmen
        * Both ends exchange public keys & agree on using the biggest (546>123)
        * Each side starts wih the private key of the other (sent using same method as RSA)
        * In the middle use the public key agreed on
        * In the end each end add it's own private key
      * Hybrid
  * Authentication
    Encrypts username & password only not the data
    * Pre-key
      * MD5
      * SHA
    * PKI ()
  * Integrity
    (Hashing)
    * HMAC-MD5
    * HMAC-SHA
  * Anti-Replay
    * Uses session numbers to avoid the same information to be sent more than once
  * Secured
  
  ## Cons of VPN
  
  * Works only with static routing no dynamic routes allowed 
  
  
  --- 
  
  # VPN encapsulation
  
  * Uses Layer 3
  * AH: Hides the Data from the ISP
  * ESP: hides the data, source & destination IP (uses the public addresses)
  
  
  
  ---
  
  # GRE (VPN)
  Generic routing Encps
  
  ## Implementing Site To Site GRE tunnel (VPN)
  
  * Add a static route from the external network of the source to the external network of the destination (**NOT THE INSIDE**)
  * Apply the following on the entry routers (not the middle ones)
  ```
  Router(config)#interface tunnel 0
  Router(config-if)#ip address 30.0.0.1 255.0.0.0
  Router(config-if)#no shutdown
  # source: interface on the external used to transport
  Router(config-if)#tunnel source gigabitethernet 0/0 
  # destination: address of the destination's external port
  Router(config-if)#tunnel destination 11.0.0.2
  ```
  * Add routing on the tunnels (without passing by the middle network)
'''
tags: [
  "CISCO"
]
isStarred: false
isTrashed: false
