  # PPP
  Point-to-Point Protocol
  
  A layer 2 encapsulation commonly used on leased lines, which supports authentication, compression, error checking and a logical multilink interface
  
  * Serial port only (PPPoE is used for PPP on Ethernet links)
  * IEEE
  * Alternative to HDLC (cisco)
  * Must be configured (not default)
  * Terminology:
    * LCP (Link Control Protocol) [check below]
    * NCP (Network Control Protocol) [check below]
    * LQM (Link Quality Management) [check below]
  * Link traffic consists of any possible combination of LCP, NCP, and network layer protocol packets. 
  * Features (LCP Options): When options are configured, a corresponding field value is inserted into the LCP option field
    * Authentication protocol 
      * Username 
      * Password
        * PAP
        * CHAP
        * CHAP PAP:
          * Uses CHAP, if it doesn't work use PAP
        * PAP CHAP:
          * Uses PAP, if it doesn't work use CHAP
    * Compression 
      * PPP uses software compression on serial interfaces 
      * It can affect system performance
      * If the traffic already consists of compressed files, such as .zip, .tar, or .mpeg, do not use this option
      * Available algorithms
        * Stacker 
        * Predictor
    * Error detection and correction [doesn't work well with multiple links] 
      * Uses the Quality and Magic Number options to help ensure a reliable, loop-free data link. 
        * The Magic Number field:
          * Helps in detecting links that are in a looped-back condition. 
          * Before configuring the Magic-Number, it is transmitted as zero.
          * Magic numbers are generated randomly at each end of the connection.
    * PPP Callback: PPP callback is used to enhance security. 
      * A router can act as a callback client or a callback server. 
        * The client makes the initial call, requests that the server call it back, and terminates its initial call. 
        * The callback router answers the initial call and makes the return call to the client based on its configuration statements
    * Multiple links support (Same logic as Etherchannel) [doesn't work well with error detection and correction] 
      * Provides load balancing over the router interfaces that PPP uses. 
      * Multilink PPP (MP, MPPP, MLP, or Multilink) provides a method for spreading traffic across multiple physical WAN links 
      * It provides: 
        * Packet fragmentation and reassembly
        * Proper sequencing
        * Multivendor interoperability
        * Load balancing on inbound and outbound traffic
      * MPPP fragments packets and sends these fragments simultaneously over multiple PPP links to the same remote address. 
      * The multiple physical links come up in response to a user-defined load threshold. 
      * MPPP can measure the load on just inbound traffic, or on just outbound traffic, but not on the combined load of both inbound and outbound traffic
  
  
  
  
  ---
  # LCP
  Link Control Protocol
  
  A protocol used by PPP to setup, maintain and teardown a connecion
  
  * Functions within L2 
  * Has a role in: 
    * Establishing 
    * Configuring
    * Testing the data-link connection 
  * Establishes the point-to-point link
  * Negotiates and sets up control options on the WAN data link, which are handled by the NCPs.
  * LCP provides automatic configuration of the interfaces at each end:
    * Handling varying limits on packet size
    * Detecting common misconfiguration errors
    * Terminating the link
    * Determining when a link is functioning properly or when it is failing
  * After the link is established, PPP also uses LCP to agree automatically on encapsulation formats such as
    * Authentication
    * Compression
    * Error detection
  * LCPs can negotiate modifications to the standard PPP frame structure. Modified frames, however, are always distinguishable from standard frames
  
  ## LCP operation
  
  LCP operation includes provisions for link establishment, link maintenance, and link termination. 
  
  * LCP operation uses three classes of LCP frames to accomplish the work of each of the LCP phases
    * Link-establishment frames establish and configure a link 
      * Configure-Request
      * Configure-Ack
      * Configure-Nak (Configure-Reject)
    * Link-maintenance frames manage and debug a link 
      * Frames used for testing the link
        * Echo-Request
        * Echo-Reply
        * Discard-Request
      * Frame types that provide feedback when one device receives an invalid frame (The sending device will resend the packet)
        * Code-Reject
        * Protocol-Reject
    * Link-termination frames terminate a link 
      * Terminate-Request
      * Terminate-Ack
  
  * Link Establishment: 
    * The first phase of LCP operation. 
    * This phase must complete successfully, before any network layer packets can be exchanged. 
    * During link establishment, the LCP opens the connection and negotiates the configuration parameters. 
    ```
    * => Configure-Request (link creation options, protocol, authentication parameters)
    * The responder processes the request:
      * If the options are not acceptable or recognized:
        * <= Configure-Nak (Configure-Reject) 
        * => New Configure-Request (different options)
      * If the options are acceptable
        * <= Configure-Ack
        * <=> NCP exchange
    ```
    * Authentication stage and the operation of the link is handed over to the NCP
    * When NCP has completed all necessary configurations the line is available for data transfer. 
    * During the exchange of data, LCP transitions into link maintenance.
  * Link Maintenance
    * During link maintenance, LCP can use messages to provide feedback and test the link
    ```
    * => Echo-Request
    * <= Echo-Reply
    * <=> Data exchange
    * If (=> Code-Reject)
      * <= Resend data
    ```
  * Link Termination  
    * After the transfer of data at the network layer completes, the LCP terminates the link. 
    * NCP only terminates the network layer and NCP link. 
    * The link remains open until the LCP terminates it. 
    * When the link is closing, PPP informs the network layer protocols so that they may take appropriate action.
      * If the LCP terminates the link before NCP, the NCP session is also terminated.
    * A termination request indicates that the device sending it needs to close the link. 
    * The LCP closes the link by exchanging Terminate packets. 
    ```
    * => Terminate-Request
    * <= Terminate-Ack
    ```
    * PPP can terminate the link at any time (loss of the carrier, authentication failure, link quality failure, the expiration of an idle-period timer, or the administrative closing of the link). 
  
  ---
  # NCP
  Network Control Protocols
  
  * The protocols used to negotiate the configuration of protocols being transmitted over a PPP link
  * Functions within L3
  * PPP uses a separate NCP for every network layer protocol used
    * IPv4 uses IPCP (IP Control Protocol)  
    * IPv6 uses IPv6CP (IPv6 Control Protocol) 
  * NCPs include functional fields containing standardized codes to indicate the network layer protocol that PPP encapsulates
    * 8021: IPCP
    * 8057: IPv6CP
    * 8023: OSI Network Layer Control Protocol
    * 8029: AppleTalk Control Protocol
    * 802b: Novell IPX Control Protocol
    * c021: Link Control Protocol
    * c023: Password Authentication Protocol
    * c223: Challenge Handshake Authentication Protocol
  * Each NCP manages the specific needs required by its respective network layer protocols
  * The various NCP components encapsulate and negotiate options for multiple network layer protocols
  * Verifies the authentication
  
  
  ## NCP Operation
  
  * After the LCP has configured and authenticated the basic link, the appropriate NCP is invoked to complete the specific configuration of the network layer protocol being used
  * When the NCP has successfully configured the network layer protocol, the network protocol is in the open state on the established LCP link
  * At this point, PPP can carry the corresponding network layer protocol packets
  * After the NCP process is complete, the link goes into the open state and LCP takes over again in a link maintenance phase. 
  * When data transfer is complete, NCP terminates the protocol link and LCP terminates the PPP connection
  
  * IPCP Example
    * After LCP has established the link, the routers exchange IPCP messages, negotiating options specific to IPv4. 
    * IPCP is responsible for configuring, enabling, and disabling the IPv4 modules on both ends of the link
    * IPCP options:
      * Compression: 
        * Allows devices to negotiate an algorithm to compress TCP and IP headers and save bandwidth
        * The Van Jacobson TCP/IP header compression reduces the size of the TCP/IP headers to as few as 3 bytes.
      * IPv4-Address: 
        * Allows the initiating device to specify an IPv4 address to use for routing IP over the PPP link, or to request an IPv4 address for the responder
        
  
  ---
  # Connection phases:
  * The connection passes by three phases
    * Phase 1: Link establishment and configuration negotiation (Shall we negotiate?)
      * Before PPP exchanges any network layer datagrams (such as IP), the LCP must first open the connection and negotiate configuration options
      * This phase is complete when the receiving router sends a Configure-ack frame back to the router initiating the connection
    * Phase 2: Link quality determination (Maybe we should discuss some details about quality. Or maybe not... )[optional]
      * The LCP tests the link to determine whether the link quality is sufficient to bring up network layer protocols
      * The LCP can delay transmission of network layer protocol information until this phase is complete
    * Phase 3: Network layer protocol configuration negotiation (time for NCP to negotiate higher level details)
      * After the LCP has finished the link quality determination phase, the appropriate NCP can separately configure the network layer protocols, and bring them up and take them down at any time.
      * If the LCP closes the link, it informs the network layer protocols so that they can take appropriate action
    * Closing the link
      * The link remains configured for communications until explicit LCP or NCP frames close the link, or until some external event occurs such as an inactivity timer expiring, or an administrator intervening
      * The LCP can terminate the link at any time. This is usually done when one of the routers requests termination, but can happen because of a physical event, such as the loss of a carrier or the expiration of an idle-period timer.
  
  ---
  # LQM
  Link Quality Monitor
  Monitors the quality of the link. 
  
  * If the error percentage falls below the configured threshold, the **link is taken down** and packets are rerouted or dropped
  * LQM uses a timeout so that the link does not bounce up and down
  * LCP tests the link to determine whether the link quality is sufficient to use Layer 3 protocols
  * The percentages are calculated for both incoming and outgoing directions. 
    * The outgoing quality is calculated by comparing the total number of packets and bytes sent, to the total number of packets and bytes received by the destination node.
    * The incoming quality is calculated by comparing the total number of packets and bytes received to the total number of packets and bytes sent by the destination node
  
  ---
  # PPP authentication
  
  * PAP: Password Authentication Protocol 
    * PAP is not interactive: The username and password are sent as one LCP data package rather than one PPP device sending a login prompt and waiting for a response as in some authentication mechanisms 
    * Uses 1-Way authentication
      * **Client** authenticates with **server**
      * The client controls the frequency and timing of the login attempts
        * After authentication is established, it does not re-authenticate
          * No protection from playback or repeated trial-and-error attacks
    * 2-Way hand-shake process
    * Password is sent in clear text
    ```
    * PPP link establishment phase completed
    * client => username + password (continue sending until receiving from server or the connection is terminated)
    * server checks the received credentials 
    * server <= accept/reject
    ```
  * CHAP: Challenge Handshake Authentication Protocol 
    * Uses 2-Way 
    * Conducts periodic challenges to make sure that the client still has a valid password
    * The password is variable and changes unpredictably while the link exists
      * Provides protection against a playback attack by using a variable challenge value that is unique and unpredictable
        * The challenge is unique and random => the resulting hash value is also unique and random
      * The use of repeated challenges limits the time of exposure to any single attack
    * The server is in control of the frequency and timing of the challenges
    * 3-Way hand-shake authentication process
    * Password is sent in encrypted text (md5)  
    * Debug output codes 
      * `Serial0: CHAP input code = 4 id = 3 len = 48`
        * code: 
          * 1: Challenge
          * 2: Response
          * 3: Success
          * 4: Failure
        * id: The ID number per LCP packet format
        * len: The packet length without the header
    ```
    * PPP link establishment phase completed
    * server <= challenge
    * client => md5 hash of password + challenge message
    * server checks the sent value with the expected one
      * if value matches
        * <= accept
      * if value doesn't match
        * <= reject
        * server terminates the connection...
    
    ```
    
  
  
  
  ---
  # Frame content
  
  | Flag | Address | Control | Protocol | Data | FCS | Flag | 
  |-|-|-|-|-|-|-|
  |01111110|11111111|00000011|-|-|-|01111110|
  
  * Flag (1 Byte) [01111110]
    * A single byte that indicates the beginning of a frame.
  * Address (1 Byte) [11111111]
    * The standard broadcast address. PPP does not assign individual station addresses. 
  * Control (1 Byte) [00000011]
    * Calls for transmission of user data in an unsequenced frame
  * Protocol (2 Bytes)
    * Identify the protocol encapsulated in the information field of the frame. The 2-byte Protocol field identifies the protocol of the PPP payload. 
  * Data (variable)
    * Zero or more bytes that contain the datagram for the protocol specified in the protocol field
  * FCS (2 or 4 Bytes)
    * This is normally 16 bits (2 bytes). If the receiver’s calculation of the FCS does not match the FCS in the PPP frame, the PPP frame is silently discarded
  * Flag (1 Byte) [01111110]
    * A single byte that indicates the end of a frame.
  
  
  ---
  
  # PPPoE
  
  PPP Over Ethernet
  
  * Offers the same PPP features on ethernet lines
  * Terminology
    * Dialer interface: A logical interface that points to a dialer pool
      * The dialer interface to take's it's address from the PPPoE server using IPCP (IP Control Protocol)
      * Best practise to set the MTU of a dialer interface to 1492 because PPP adds an 8 byte header and the maximum used MTU is 1500
  
  ---
  
  # Troubleshooting PPP and PPPoE
  
  * Common problems:
    * Encapsulation mismatch
    * Authentication mismatch
    * Dialer interface misconfiguration
    * Dialer pool misconfiguration
    * Clocking on physical interface (DCE/DTE clock)
  
  
  ---
  
  # Configuration
  
  ## PPP
  
  * Change the hostname
  * enter the enterface
  * change encapsulation to ppp
  * `ppp authentication (chap|pap)`
  * exit enterface
  * `(conf)#username <other-end> password <password>`
  
  * Configure PPP encapsulation
  ```
  Router(config-if)#encapsulation ppp
  ```
  
  * Configure pap client/server authentication (after activating ppp encapsulation)
  ```
  # Configure server
  ## Create a user
  RouterS(config)#username <user> (password|secret) <password>
  ## Activate pap authentication on interface
  RouterS(config-if)#encapsulation ppp
  RouterS(config-if)#ppp authentication pap
  
  # Configure client
  RouterC(config-if)#encapsulation ppp
  RouterC(config-if)#ppp authentication pap
  RouterC(config-if)#ppp pap sent-username <user> password <password> 
  ```
  
  * Configure chap authentication
  ```
  # Router1 configuration
  ## Create a username for the other router using the same password
  Router1(config)#username <Hostname_of_Router2> (password|secret) <password>
  
  ## Activate chap authentication on an interface
  Router2(config-if)#ppp authentication chap
  
  # ======================
  
  # Router2 configuration
  ## Create a username for the other router using the same password
  Router2(config)#username <Hostname_of_Router1> (password|secret) <password>
  
  ## Activate chap authentication on an interface
  Router2(config-if)#ppp authentication chap
  
  ```
  
  * Enable (or configure) PPP compression  
  ```
  Router(config-if)#compress (stac|predictor|mppc)
  ```
  
  * Enable error detection and correction
  ```
  Router(config-if)#ppp reliable-link
  ```
  
  * Multiple link aggregation (Etherchannel)
  ```
  # Multilink configuration
  ## Create a multilink
  Router(config)#interface multilink <multilink_id>
  ## Assign it an ip address
  Router(config-if)#ip address <ip_address> <mask>
  ## Activate multilink capabilities
  Router(config-if)#ppp multilink
  
  # Add physical interfaces to the multilink
  Router(config)#interface serial 0/0/0
  Router(config-if)#ppp multilink group <multilink_id>
  ```
  
  * Use link quality
  ```
  Router(config-if)#ppp quality <percentage>
  ```
  
  
  * Check the encapsulation type | used NCPs | error detection and correction status 
  ```
  Router#show interface <interface>
  ```
  
  * Show compression statistics
  ```
  Router#show compress
  ```
  
  * Show multilink configuration
  ```
  Router#show ppp multilink
  ```
  
  ## PPPoE
  
  * Configure PPPoE client 
  ```
  # Create a dialer interface
  ￼Router(config)#interface dialer <dialer_int_id>
  
  # Configure a dialer interface
  ## Use IPCP to obtain the IP address
  ￼Router(config-if)#ip address negotiated
  ## Set the MTU
  ￼Router(config-if)#ip mtu 1492
  
  # Configure PPP
  ￼Router(config-if)#encapsulation PPP
  ￼Router(config-if)#ppp chap hostname <username>
  ￼Router(config-if)#ppp chap password <password> 
  
  # Associate the dialer interface with a dialer pool
  ￼Router(config-if)#dialer pool <dialer_pool_id>
  
  # Assign an external physical interface to a dialer pool
  Router(config-if)#no ip address
  Router(config-if)#pppoe enable
  Router(config-if)#pppoe-client dial-pool-number <dialer_pool_id> 
  Router(config-if)#no shutdown
  
  # Set a TCP MSS limit on LAN interface to prevent TCP sessions from closing
  Router(config-if)#ip tcp adjust-mss 1452
  ``` 
  
  * Configure PPPoE server [not a ccna objective]
  ```
  # Create a username and password for users to log-in to
  Router(config)#username <username> (password|secret) <password> 
  
  # Create an IP address pool
  Router(config)#ip pool local pool <address_pool_name> <start_ip> <end_ip>
  
  # Create a Virtual Template interface
   Router(config)#interface virt <virt_template_id>
  Router(config-if)#ip address <ip> <mask>
  Router(config-if)#peer default ip address pool <address_pool_name>
  Router(config-if)#ppp authetication chap callin
  
  # Create a Boradband access group
  Router(config)#bba-group pppoe <bba_group_name>
  # Assign a virtual template to the BBA Group
  Router(config-##)#virtual-template <virt_template_id>
  
  # Configure the entery physical address
  Router(config-if)#no ip address
  Router(config-if)#media-type rj45
  Router(config-if)#pppoe enable group <bba_group-name>
  
  
  ```
  
  * Set the clock rate on the DCE
  ```
  Router(config-if)#clock rate <clock_value>
  ```
  
  * Show pppoe information
  ```
  Router#show pppoe session 
  ```
  
  * Show DCE/DTE
  ```
  Router#show controllers serial <int_id>
  ```
'''
