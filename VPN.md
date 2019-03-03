  # VPN
  Virtual Private Network
  
  A logically defined network where data flows through a tunnel between two sites and where the date might be encrypted, allwing the data to be securely sent across an untrusted network (e.g. inernet)  
  
  * Benefits:
    * Can use common broadband technologies (DSL, cable...)
    * Can scale to many connections (ie. new connection just need internet access)
    * Can securely transmit data over an untrusted network
  * Categories (types):
    * Site to site VPN
      * Connects 2 sites using routers acting as endpoints
      * Transparent to the end user
      * Connect to a LAN from the outside
      * External IP address must be fixed
    * Remote access 
      * VPN client
        * Needs a special application
      * SSL VPN (Secure Sockets Layer VPN) 
        * Uses a browser directly
  
  ## Pros of VPN
  1. **Confidentiality** (Encryption)
     * Symetric: *Uses a private key*
       * Types:
         * DES (64bit)
         * 3DES
         * AES (192bit)
     * Asymetric *Uses a private & a public key*
       * Types:
         * RSA
           * (web auth): Client sends a request, server sends it's public key, client sends his private key encrypted with the public key to open the account
         * Diffe-Hallmen
           * Both ends exchange public keys & agree on using the biggest (546>123)
           * Each side starts wih the private key of the other (sent using same method as RSA)
           * In the middle use the public key agreed on
           * In the end each end add it's own private key
         * Hybrid
  2. **Authentication**: *Encrypts username & password only not the data*
     * Pre-key
       * MD5
       * SHA
     * PKI ()
  3. **Integrity** *Hashing*
     * HMAC-MD5
     * HMAC-SHA
  4. **Anti-Replay**
     * Uses session numbers to avoid the same information to be sent more than once
  5. **Secured**
  
  ## Cons of VPN
  
  * Works only with static routing no dynamic routes allowed 
  
  
  ---
  # IPsec
  
  * A type of VPN that can provide confidentiality (encryption), data integrity (check that data hasn't been modified using hashing algorithms), authentication (login) and anti-replay protection (avoid having someone capturing a successful login token and reusing it to loging themselves) 
  * IPsec is a tunnel inside a tunnel
    1. Outer side: IKE (Internet Key Exchange) phase 1 tunnel
       * The outer tunnel in an IPsec connection (also known as an ISAKMP tunnel), in which the security parameters for the inner tunnel are negotiated
    2. Inner side
        * The inner tunnel in an IPsec connection (also known as an IPsec tunnel), in which traffic is protected
  * Can only protect unicast IP traffic 
    * Multicast, broadcast and non IP traffic are encapsulated in a GRE tunnel then inside an IPsec tunnel
  
  --- 
  
  # VPN encapsulation
  
  * Uses Layer 3
  * AH: Hides the Data from the ISP
  * ESP: hides the data, source & destination IP (uses the public addresses)
  
  
  
  ---
  
  # GRE (VPN)
  Generic routing Encpsulation
  
  * A logical connection that can encapsulate a wide variety of data types
  * A type of tunnel that can encapsulate multiple L3 protocols
  * GRE itself is not secure
  * GRE is a unicast IP packet
  * Common issues 
    * Source IP address for tunnel not reachable by far-end router
    * Destination IP address for tunnel not reachable by local router
    * GRE traffic blocked by ACL 
    * Fragmentation caused by inconsistent MTU settings
    * Recursive routing
      * Best route to tunnel destination is through the tunnel itself
      * A condition on a GRE tunnel that occurs when the IP route used to get to the tunnel's destination IP address uses the tunnel itself
  
  ---
  
  # DMVPN fundamentals
  Dynamic Multipoint VPN
  
  * Allows VPN tunnels to be setup and torn down on an as-needed basis
  
  
  ---
  # mGRE 
  Multipoint GRE
  
  * Allows a single router interface to have multiple GRE tunnels
  * NHRP (Next Hop Resolution Protocol): Allows an interface configured for mGRE to discover the IP address of the device at the far end of a tunnel
  
  
  ---
  # Configuration
  
  * Implementing Site To Site GRE tunnel (VPN)
  ```
  # Create a tunnel interface (on both ends of the tunnel)
  Router(config)#interface tunnel <tunnel_int_id>
  ## Configure addressing
  Router(config-if)#ip address <ip_add> <mask>
  ## Configure a tunnel source (either use local loopback interface, or the interface taking to the tunnel end)
  Router(config-if)#tunnel source <interface>
  ## Configure a tunnel destination (either use destination's loopback interface or the destinations port facing the source)
  Router(config-if)#tunnel destination <ip_add>
  ## Bring up the interface
  Router(config-if)#no shutdown
  
  # Configure routing between both ends
  ## Make sure that both ends can reach eachother (using the physical interfaces ip addresses)
  ## Add a route from the external network of the source to the external network of the destination (**NOT THE INSIDE**) 
  ## Add routing on the tunnels (without passing by the middle network)
  
  
  
  ```
  
  
  
'''
tags: [
  "CISCO"
