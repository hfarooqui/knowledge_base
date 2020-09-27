
# Python

### List comprehensions

### Generators

### Lambda

### List vs Tuple

### Hashtable


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


Python:

Metaclasses:
…

Decorators:
…

GIL:
…

Itertools:
...

python delegates thread scheduling to the host operating system
Python is multi-threaded. What python is not is concurrent. There’s a difference

Format phone number:
import re
str = re.sub("\D", "","555b999x8888")
if len(str) != 10:
    str = str[:3] + '-' + str[3:6]+ '-' + str[6:10]
    print str
else:
    print "invalid number"

List operations
``
# Output: [4, 16, 36, 64]

from functools import reduce
product = reduce((lambda x, y: x * y), [1, 2, 3, 4])
# Output: 24

number_list = range(-5, 5)
less_than_zero = filter(lambda x: x < 0, number_list)
# Output: [-5, -4, -3, -2, -1]

Palindrome
def isPalindrome(str):
  
  if len(str) < 2:
    return True
    
  if str[0] == str[-1]:
    newstr = str[1:-1]
    print newstr
    isPalindrome(newstr)
    return True
  else:
    return False
  
palndrome = isPalindrome("qwertyuiuytrewq")
print palndrome


Max repeated characters
def longestrepeatedcharacters(s):
    start = maxLength = 0
    usedChar = {}
    max_len = 1
    max_char = s[0]
    
    usedChar
    for i in range(len(s)):
        if s[i] in usedChar:
            
            usedChar[s[i]] = usedChar[s[i]] + 1
            
            if usedChar[s[i]] > max_len:
              max_len = usedChar[s[i]]
              max_char = s[i] 
        else:  
            usedChar[s[i]]  = 1
            #print s[i]
            #print usedChar

    return max_len, max_char
    
print lengthOfLongestSubstring("bacdefghi")
