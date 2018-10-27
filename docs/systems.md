## Shell Commands

#### User
- Create user: usermod <username>
- Set Password: passwd <username>
- Lock an account: passwd -l <user>
- Unlock an account: passwd -u <user>
  
#### VirtualBox
- Enable ssh to vbox from mac: Add "Bridge Adapter" network to VM

-Add fixed IP to vm:
sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
TYPE=Ethernet
BOOTPROTO=none
NAME=enp0s3
UUID=3f7bcced-7d57-4f5a-9e9a-8aa4b9c9ec4a
DEVICE=enp0s3
NETMASK=255.255.255.0
ONBOOT=no
ONBOOT=yes
IPADDR=192.168.86.80

- Enable passwordless access to vm: ssh-copy-id hfarooqui@192.168.86.115 (copies public key from host to authorized_keys on vm)
