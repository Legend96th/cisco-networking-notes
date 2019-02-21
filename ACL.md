  # ACL
  Access Control List
  
  * Can contain multiple entries
  * Each Entry can deny or permit something
  * Can be applied inbound or outbound
    * Only 1 ACL per direction is allowed on the same interface
  * The entries are processed top-down
  * Specefic policies (entries) are placed near the top and general ones near the bottom
  * **There's an implicit "DENY ANY" statement at the bottom of an ACL**
  * **Standard** ACLs are better placed **near the destination**
  * **Extended** ACLs are better placed **near the source**
  * Can filter traffic
  * Can match traffic
  * Types
    * Standard: Can filter traffic based on source IP address
      * Numbered (1-99)
      * Named
    * Extended: Can filter traffic based on source and destination IP address
      * Numbered (100-199)
      * Named
  
  
  ---
  # Numbered
  
  ## Standard
  To create a standard numbered ACL, give it an id from 1 to 99
  * Create a policy in general conf
    * Policies are applied sequentially
    * A policy "n" **cant** overwrite a policy "n-1" 
      * Same logic with react router picking on routes
      * Make special policies first then general ones in the end
    * There are 2 types of policies
      * Deny: block traffic
      * Permit: allow traffic
    * If we start with deny we MUST allow otherwise everything is denied
    * Specify a network address (with mac) or use keyword "**any**"
  * Apply a policy on an interface
    * Apply a policy using it's ID
    * Specify traffic flow ("in" or "out") of the router
  
  ---
  
  # IPv6 ACLs
  * Same as IPv4 ACLs
  * Standard IPv6 ACLs can filter traffic based on source and destination IP address
  * Only named ACLs
  * IPv6 NDP (Neighbor Discovery Protocol) messages are implicitly allowed to pass ACLs **but router advertisement messages are NOT allowed to pass** so don't forget to allow them
    
  
  ---
  
  # Configuration
  
  ## 1.Numbered
  
  ### Standard
  ```
  # Create an ACL
  Router(config)#access-list <1-99> (permit|deny) (any|<network_ip_address> <wildcard_mask>|host <host_ip_address>)
  
  # This command is implicitly added denying anything not permited on the ACL
  ## Router(config)#access-list <1-99> deny any
  
  # Apply the ACL on an interface
  Router(config-if)#ip access-group <1-99> (in|out)
  ```
  
  ### Extended
  ```
  # Create an ACL
  Router(config)#access-list <100-199> (permit|deny) (ahp|eigrp|esp|gre|icmp|ip /*Any Internet Protocol*/|ospf|tcp|udp) (any|<network_ip_address> <wildcard_mask>|host <host_ip_address>) (any|<network_ip_address> <wildcard_mask>|host <host_ip_address>)
  
  # This command is implicitly added denying anything not permited on the ACL
  ## Router(config)#access-list <100-199> deny any
  
  # Apply the ACL on an interface
  Router(config-if)#ip access-group <100-199> (in|out)
  
  ```
  
  **Note:**
  If TCP or UDP are specified, the exact port number or a range of ports can be selected
  
  
  ## 2.Named
  
  ### Standard
  
  *Same as extended acl configuration, only specify source of traffic*
  
  ### Extended
  
  ```
  # Create an ACL
  Router(config)#ip access-list (extended|standard) <name>
  
  # Add policies
  Router(config-ext-nacl)#[<sequence_number>] (permit|deny) (ahp|eigrp|esp|gre|icmp|ip|ospf|tcp|udp)
  
  # Apply the ACL on an interface
  Router(config-if)#ip access-group <acl_name> (in|out)
  ```
  
  **Notes:**
  * Sequence numbers can be added to insert a policy at a specified position to be applied before or after other policies
  * If TCP or UDP are specified, the exact port number or a range of ports can be selected
  
  # IPv6 ACL configuration
  
  * IPv6 ACL configuration is the same as named IPv4 ACLs, just replace:
    * `ip` with `ipv6`
    * Applying the acl on an interface 
    ```
    Router(config-if)#ip traffic-filter <acl_name> (in|out)
    ```
  * Allow router advertisement and solicitation messages
  ```
  Router(config-ipv6-acl)#permit icmp any any router-advertisement
  Router(config-ipv6-acl)#permit icmp any any router-solicitation
  ```
'''
