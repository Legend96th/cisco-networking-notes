createdAt: "2018-09-09T10:05:49.520Z"
updatedAt: "2019-01-24T10:09:01.205Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "Password recovery"
content: '''
  # Password recovery
  
  To perform a password recovery we must alter the normal boot procedure 
  (ROMMON => Flash => NVRAM => RAM ) 
  and bypass from the flash directly to the RAM skipping the NVRAM.
  (ROMMON => Flash =>  >>   => RAM ) 
  After booting this way, the devices is without configuration (no config since it's stored in NVRAM). To restore the previous configuration, a restauration from a backup must be copied
  The config is backed  up & restored using a tftp server.
  
  
  
  0x2102: normal boot from flash
  0x2142: bypass NVRAM boot
  
  
  ## Bypass NVRAM
  * Phyisically reboot the device
  * Interupt the boot with Ctrl-C
  ```
  confreg 0x2142
  reset
  ```
  
  ## Backup / restore config
  To backup / restore the config, use a tftp server
  To backup copy either the running-config or startup-config to the tftp server
  To restore copy the tftp config to the running config, at this stage u can do live modification (like change password) then save ur configuration to the NVRAM.
    *Note*: If you restore to startup config with a forgotten password, it'll reboot with that password
  
  
  ---
  * ROM contains: 
    * POST
    * Boot Strap
    * ROMMON: Router's BIOS
  
  ---
  
  # Boot sequence 
  
  * POST (Power On Self-Test): Checking internal components & performing diagnostic tests
  * Execute bootstrap code: Locate the IOS & replaces itself with it
  * Locate IOS
  * Load IOS
  * Locate the configuration
  * Load the configuration
  * Execute the configuration 
  
  --- 
  
  # Boot options
  
  * Controlled by the router's **configuration register**
    * Reload into ROM monitor mode: A very basic OS residing in a router which is used for tasks such as troubleshooting and password recovery
    * Boot from the **first** IOS image in flash
    * Get image loading instructions from configuration in NVRAM
  
  ### Configuration Register:
  * 16 bits:
    * **0-3**: Boot field
    * 4: Not used
    * 5: Console speed
    * 6: Ignore NVRAM
    * 7: OEM bit
    * 8: Break disabled
    * 9: Seconday bootstrap
    * 10: IP broadcast with all 0s
    * 11-12: Console Speed
    * 13: Boot default ROM if network boot fails
    * 14: IP broadcast without network numbers
    * 15: Enable diagnostic messages
  
  |Configuration register|Description|
  |=|=|
  | 0x0 | Boot into ROM monitor mode |
  | 0x1 | Boot from first image in flash |
  | 0x2 - 0xF | Get image loading instructions from configuration in NVRAM |
  
  0x2102: normal boot from flash
  0x2142: bypass NVRAM boot
'''
tags: []
isStarred: false
isTrashed: false
