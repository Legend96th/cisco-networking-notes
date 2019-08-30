  # Security
  
  * Physical installation
    * Secure equipment in seprate room
    * Using video monitoring or wireless webcam or fingerprint access
    * Replicate the devices (switches) in case of failiur
  
  ---
  
  # General security mechanisms
  
  ## Securing the CLI
  
  * Implement authentication checks against any access to the CLI 
    * Authenticate an implement passwords against connections to the console
      * Static passwords on individual TTY lines
      * Unique passwords for unique users [[recommended]]
    * Implement passwords to authenticate privileged EXEC sessions
      * By default, authenticated users have access to priveleged EXEC mode
      * Two methods exist:
        1. Enable password
            * Password stored in clear text
            * Password can be encrypted using `service password-encryption`
              * This provides a weak Type-7 encryption algorithm
        2. Enable secret
            * Password is MD5 hashed and stored
            * Has priority over `enable password`
  * Place infrastructure equipments in highly secured rooms
  * Restric remote access to CLI from authorized subnets
  * Prevent unauthorized password-recovery attempts
  * If you are sending the configuration of a device, use `show tech-support` which will automatically remove all the passwords
    * warning: this has a lot more data and can take some time
  
  
  ---
  
  * Types of virus
    * Virus: Anything that you execute & it affects you (user interaction is a must)
    * Worm: The user isn't required to interact; eg: if a usb is plugged it's attacked
    * HorseTrogan: a malware is hidden inside an application, once it's installed the malware is spread
  * Reconaisances
    * Packet sniffing
    * Ping sweep
    * Port scan
    * Access attack
      * Brute force
      * Dictionnary attack
  
  ---
  
  * Tools
    * Yersinia: create fake DHCP/DNS
    * Blackeye: fake webpages
  
  
'''
