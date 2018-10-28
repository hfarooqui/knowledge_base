## Shell Commands

#### User
- Create user: usermod <username>
- Set Password: passwd <username>
- Lock an account: passwd -l <user>
- Unlock an account: passwd -u <user>
  
#### VirtualBox
- Enable ssh to vbox from mac: Add "Bridge Adapter" network to VM

- Add fixed IP to vm:  <br />
sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3  <br />
TYPE=Ethernet <br />
BOOTPROTO=none  <br />
NAME=enp0s3  <br />
UUID=3f7bcced-7d57-4f5a-9e9a-8aa4b9c9ec4a  <br />
DEVICE=enp0s3  <br />
NETMASK=255.255.255.0  <br />
ONBOOT=no  <br />
ONBOOT=yes  <br />
IPADDR=192.168.86.80  <br />

- Enable passwordless access to vm: ssh-copy-id hfarooqui@192.168.86.115 (copies public key from host to authorized_keys on vm)

#### Install
Java (OpenJDK 8): sudo yum install java-1.8.0-openjdk-devel
Enable (Jenkins) repository: curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
Add repository to system: sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

#### Definitions
What are the functional differences between .profile .bash_profile and .bashrc?
