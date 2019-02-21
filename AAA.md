  # AAA
  Authenticate Authorization & Accounting
  
  * Authentication: Who are you?
  * Authorization: What are you allowed to do?
  * Accounting: What did you do?
  
  * Similar to LDAP
  
  * Protocols
    * Raduis 
      * IEEE
      * */UDP 
        * 1812|1645 Authentication
        * 1813|1646 Accounting
      * Runs a single process for all it's functionalities
      * One-way authentication
      * Encrypts only the password
      * Better accounting
    * Tacacs 
      * Developped by cisco now open to public
      * 49/TCP
      * Uses a different process for the different functionalities
      * Two-way authentication
      * Encrypts the entire packet (name & password)
    
  --- 
  ## Configuration
    
  ```
  # Enable AAA
  Switch(config)#aaa new-model
  # Configure a single AAA server
  Switch(config)#(radius|tacacs)-server host <ip_address> key <key>
  # Configure a group of AAA servers
  Switch(config)#aaa group server (radius|tacacs+) <group_name>
  # Add servers
  Switch(config-sg-radius)#server <ip_address>
  # Add authentication lists (sources)
  Switch(config)#aaa authentication login (default|<name>) group <group_name> <backup_source_of_auth>
  ```
'''
