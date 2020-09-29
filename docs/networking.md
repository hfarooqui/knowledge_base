**Networking:**

## Routing

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

## Firewalls

### Reverse proxy
### Forward proxy vs Reverse proxy

## OSI Model

### Application Layer (7)
- This layer is used as a `means of communication between the operating system, the application, and the end user`
- Closest to the end-user.
- Also called `interface layer` because functionalities implemented at this level are the ones that users interact with directly. 
- For example, Firefox, Skype and your local email client are all application-level software.
- Application level error occur here
- Protocols - HTTP, TELNET, SMTP, and DNS

### Presentation Layer (6)
- Used to create a `standard for communication between the application and network formats (translator)`
- Handles `encryption, data compression` and a few other things
- Also called the `translation layer`. “translates” data going to and from the application layer. Because data going to and from the application layer is often not in the format required to be transported over the network, it needs to be translated and “presented properly” to the next layer.
- `Translates data from the network format to the application format or vice versa` 
- An example of this would be how secure communication is implemented in HTTPS: data is encrypted before transmission and decrypted upon arrival

### Session Layer (5)
- Used to `create and manage sessions across a network`
- Manages “sessions” between machines. F
- For two devices on the network to communicate with each other, a “session” needs to be created to ensure `authentication and security`. This layer sets up, maintains and terminates these sessions.
- An example of this would be `Remote Desktop`. If a user reported they were unable to connect to your application server using Remote Desktop or 
- Protocols - NetBIOS and RPC are some of the protocols used on this layer

### Transport Layer (4)
-  `traffic control` of networks
- Controls host-to-host data transfer 
- Dictates `how much data to send` at once, `how fast to send it` and `who to send it to`
- Takes care of the data segmentation and reassembly, as well as flow and error control
- `Data segmentation means dividing up the data into smaller chunks prior to transmission`. It is needed when the data being sent is larger than the maximum transmission unit supported by the network, or anytime when sending smaller amounts of data at a time is beneficial. Upon receiving segmented data, the transport layer protocols will also take care of `reassembling` the data back to its original state.
- `Flow and error control` is also an important part of ensuring the proper functioning of a network. When too much data is being sent at a time, network congestions may happen. Then, it is up to the transport layer protocols to decide which data to send first and in what order to send them in order to achieve maximum overall efficiency. Still, sometimes, data packets arrive corrupted or out of order. `How to recover original data if corrupted data arrives` is also the task of this layer.
- Protocols -  TCP, UDP, ARP

`Ensures firewall validity`
Load balancer
Think of the Transport layer as a taxi driver. His or her job is to get you where you need to go and to make sure you arrive safely.
The Transport layer `provides flow control and error handling on a network`. Like the taxi driver making sure you arrive safely, the Transport layer `makes sure that all transmissions were successful`. If you get into a wreck in the taxi, that's an error by the taxi driver!
The Transport layer also makes sure that all packets and transmissions are error free. 


### Network Layer (Routers)
- Handles `network addressing and routing.` 
-`translates IP addresses into MAC addresses, or computer names to MAC addresses`. In a nutshell you would be troubleshooting mostly routers on the Network layer.
- `Post office`of networks. 
- Takes care of the routing data from its source to its destination
In order to get from spot A to spot B on any complex network, data needs to be transmitted across the network. But where is spot B? How does the 
- `IP protocol uniquely identifies devices on the network so that the destination of each transmission can be located`
- In summary, TCP is the data. IP is the Internet location GPS.
Protocols - ICMP, IP, LDAP

### Data-link Layer (Switches)
- The Data-Link layer is used to `turn packets into bits`, and vice versa. This layer also `transmits data across a physical network link`. 
- Common devices on this layer are hubs, switches, NICs, and bridges.
- Once the network layer identifies where to send the data, the data link layer `takes care of the data transfer between neighbouring network elements`. It `ensures that the data transfer is error-free over the physical layer`
- The functionality implemented in this layer includes physical addressing, framing, and error and flow control.
- `Physical addressing` refers to the addressing after a packet has arrived at the local network. `To the Internet at large, machines on the same local network may have the same IP address`. If that is the case, MAC addresses must be used to locate the right recipient of the message.
- `Framing` is a way to make sure that `sets of bits that are transmitted are understandable to the receiver`. This is accomplished by attaching special bit patterns to the beginning and end of the frame.

Protocols - Synchronous Data Link Protocol (SDLC), High-Level Data Link Protocol (HDLC), Serial Line Interface Protocol (SLIP)

###  Physical Layer
- `Lowest layer` of the OSI model - `Hardware layer`
- Responsible for the `actual transfer of bits “on the wire”`. 


I personally start at the physical layer most of the time because that is where I tend to find the majority of the issues. "The cleaning person knocked a cable lose" or "I moved my tower to the other side of my desk".