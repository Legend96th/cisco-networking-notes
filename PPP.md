  # PPP
  Point-to-Point Protocol
  
  A layer 2 encapsulation commonly used on leased lines, which supports authentication, compression, error checking and a logical multilink interface
  
  * Serial port only
  * IEEE
  * Alternative to HDLC (cisco)
  * Must be configured (not default)
  * Features
    * Authentication
      * Username 
      * Password
        * PAP: Password authentication protocol 
          * Uses 1-Way authentication
            * **Client** authenticates with **server**
          * 2-Way hand-shake process
          * Password is sent in clear text
        * Chap: Challenge-handshake authentication protocol 
          * Uses 2-Way authentication
            * 
          * 3-Way hand-shake process
          * Password is sent in encrypted text (md5)
    * Compression
    * Error detection and correction [doesn't work well with multiple links]
    * Multiple links support (Same logic as Etherchannel) [doesn't work well with error detection and correction]
  * Terminology:
    * LCP (Link Control Protocol): A protocol used by PPP to setup, maintain and teardown a connecion
    * NCPs (Network Control Protocols): The protocols used to negotiate the configuration of protocols being transmitted over a PPP link
  
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
  RouterS(config-if)#ppp authentication pap
  
  # Configure client
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
  
  * Check the encapsulation type | used NCPs | error detection and correction status 
  ```
  Router#show interface <interface>
  ```
  
  * Show compression statistics
  ```
  Router#show compress
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
  ￼Router(config-if)#mtu 1492
  
  # Configure PPP
  ￼Router(config-if)#encapsulation PPP
  ￼Router(config-if)#ppp chap hostname <username>
  ￼Router(config-if)#ppp chap password <password> 
  
  # Associate the dialer interface with a dialer pool
  ￼Router(config-if)#dialer pool <dialer_pool_id>
  
  # Assign a physical interface to a dialer pool
  ￼Router(config-if)#pppoe-client dial-pool-number <dialer_pool_id> 
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
