  # Remote access
  
  * TTY (Teletype) line: A software link between the person issuing the commands and the CPU
  * TTY line types:
    * CTY (Console Teletype): Console port
    * TTY: Provides CTY alike access to multiple devices from a single one (one router used to access other equipments)
    * VTY: Virtual TTY used for remote access
    * Aux: Used to connect a modem to an equipment to remotely dial-up and configure it (used as a backup)
  * Check enabled protocols:
    * `show line vty <num>`
    * Check "Allowed <input|output> transports are ..."
  * Change enabled protocols:
  ```
  Router(config-line)#transport input ?
    all     All protocols
    none    No protocols
    ssh     TCP/IP SSH protocol
    telnet  TCP/IP Telnet protocol
  Router(config-line)#transport output ?
    all     All protocols
    none    No protocols
    ssh     TCP/IP SSH protocol
    telnet  TCP/IP Telnet protocol
  ```
  ---
  
  # Telnet
  
  * Enabled by default when setting 
    * A login password on the "line vty"
    * An enable password
  * Exchanges data (commands and results) in clear text
  
  ---
  
  # SSH
  Secure Shell
  
  * SSH uses an RSA key to encrypt data
    * SSH V1: n = 512 -> 767
    * SSH V2: n = 768 -> ~~~ 
  * Requires IOS images that support:  
    * SSH V1: IPSec DES
    * SSH V2: IPSec 3DE
    * *Typically look for a "k9" in IOS image name*
    
  * SSH server configuration:
    * Add a hostname
    * Add a domain name
    * Add an RSA key
    * Set the SSH version to version 2
    * Add users with privilages & passwords
    * Add a line vty
    * Choose SSH as an input transport method
    * Set the username & password database to local
  
'''
