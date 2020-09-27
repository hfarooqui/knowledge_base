**Networking:**

### Routing

Routing


Your router will have to keep track of that AND tag the outgoing packets with the true IP address that your Internet Service Provider has given to your modem. I’ll call that the external IP address. How does the router do that? That’s the question.
 The router takes your computer’s local IP address out of the packet’s Source Address and puts it in a table. It then puts the external IP into the packets Source Address space. The router also copies the Destination Address IP from the packet and puts it in the table associated with your local IP



When the packet comes back from that server somewhere out on the Internet, the Destination Address IP is now your external IP and the Source Address IP is now the IP address of the server sending you a packet. (Note: that is the IP address of Telus.com – not my home IP address.)
Think of it like a letter. You send a friend a letter and the return address is yours, and the send-to address is theirs. They write a letter back and the return address is theirs and the send-to address is yours. See how that works? We should write more letters.

Well, the router looks at the Source Address IP of the incoming packet and looks it up in the table as a former Destination Address IP. When it finds it, the router says, “Aha! Guy’s computer sent a packet to that IP address. His computer must be waiting for a reply! Here’s Guy’s local IP address so I’ll pull out the external IP address, pop his local IP address in and send it on its way!” That’ll do router, that’ll do.

You can imagine, with how many thousands of packets travel in and out of your home every minute, how fast this sorting process has to be! It happens so fast, you never even notice the fact that at one moment the router is talking to the Mac, then the laptop, then maybe the Mac again, and then the PC. Miracles everywhere – just stop and notice.

LDAP, Lightweight Directory Access Protocol, is an Internet protocol that email and other programs use to look up information from a server.
"LDAP-aware" client programs can ask LDAP servers to look up entries in a wide variety of ways
LDAP is not limited to contact information, or even information about people. LDAP is used to look up encryption certificates, pointers to printers and other services on a network, and provide "single sign-on" where one password for a user is shared between many services. LDAP is appropriate for any kind of directory-like information, where fast lookups and less-frequent updates are the norm.
LDAP is a protocol to talk to directory services database (like active directory)
Initially enabled Internet applications and machines to access X.500(X.500 is a series of computer networking standards covering electronic directory services)

### Firewalls

### Reverse proxy
#### Forward proxy vs Reverse proxy

### NW layer 

### OSI Model

Application Layer (7)
This layer is used as a means of communication between the operating system, the application, and the end user
Application level error occur here
HTTP, TELNET, SMTP, and DNS

Presentation Layer (6)
The Presentation layer is used to create a standard for communication between the application and network formats (translator)
This layer handles encryption, data compression and a few other things
An example of this layer is your wireless router at home using WEP, WPA or another type of encryption. 
The Presentation layer is translating the encryption on your wireless network.

Wired Equivalent Privacy (WEP) is the most widely used Wi-Fi security algorithm in the Despite revisions to the algorithm and an increased key size, over time numerous security flaws were discovered in the WEP standard and, as computing power increased, it became easier and easier to exploit them. As early as 2001 proof-of-concept exploits were floating around and by 2005 the FBI gave a public demonstration (in an effort to increase awareness of WEP’s weaknesses) where they cracked WEP passwords in minutes using freely available software.world. 
Wi-Fi Protected Access (WPA) - Replacement for WPA
Despite Message integrity checks and Temporal Key Integrity Protocol (TKIP)/Advanced Encryption Standard (AES) (employs per packet keys system) it is vulnerable to intrusion.
Wi-Fi Protected Access II (WPA2) - WPA superseded by WPA2
Mandates use of AES algorithms and the introduction of CCMP (Counter Cipher Mode with Block Chaining Message Authentication Code Protocol) as a replacement for TKIP

Session Layer (5)
The Session layer is used to create and manage sessions across a network.
An example of this would be Remote Desktop. If a user reported they were unable to connect to your application server using Remote Desktop or apache.conf is not set correctly, then you might want to start your efforts at this layer.
NetBIOS and RPC are some of the protocols used on this layer.

Transport Layer (4)
Ensures firewall validity
Load balancer
Think of the Transport layer as a taxi driver. His or her job is to get you where you need to go and to make sure you arrive safely.
The Transport layer provides flow control and error handling on a network. Like the taxi driver making sure you arrive safely, the Transport layer makes sure that all transmissions were successful. If you get into a wreck in the taxi, that's an error by the taxi driver!
The Transport layer also makes sure that all packets and transmissions are error free. Some of the protocols used on this layer are TCP, NetBIOS, RARP, ARP, and NetBEUI.

Network Layer (Routers)
This layer handles network addressing and routing. It translates IP addresses into MAC addresses, or computer names to MAC addresses. In a nutshell you would be troubleshooting mostly routers on the Network layer.
Protocols - ICMP, IP, LDAP
Data-link Layer (Switches)
The Data-Link layer is used to turn packets into bits, and vice versa. This layer also transmits data across a physical network link. Common devices on this layer are hubs, switches, NICs, and bridges.
In summary, TCP is the data. IP is the Internet location GPS.

Protocols used TCP, UDP, ARP
Physical Layer
I personally start at the physical layer most of the time because that is where I tend to find the majority of the issues. "The cleaning person knocked a cable lose" or "I moved my tower to the other side of my desk".