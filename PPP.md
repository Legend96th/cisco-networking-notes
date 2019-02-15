createdAt: "2018-09-11T13:15:01.741Z"
updatedAt: "2018-10-24T09:51:19.735Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "PPP"
content: '''
  # PPP
  Point to Point Protocol
  
  
  * Serial port only
  * IEEE
  * Alternative to HDLC (cisco)
  * Must be configured (not default)
  * Supports
    * Authentication
      * Username 
      * Password
        * PAP: Password authentication protocol 
          * 2 way hand-shake process
          * Password is sent in clear text
        * Chap: Challenge handshake authentication protocol 
          * 3 way hand-shake process
          * Password is sent in encrypted text (md5)
    * Compression
    * Error correction
  
  ## Config
  * Change the hostname
  * enter the enterface
  * change encapsulation to ppp
  * `ppp authentication (chap|pap)`
  * exit enterface
  * `(conf)#username <other-end> password <password>`
'''
tags: []
isStarred: false
isTrashed: false
