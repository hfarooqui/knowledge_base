## Shell Commands

#### User
- Create user: usermod <username>
- Set Password: passwd <username>
- Lock an account: passwd -l <user>
- Unlock an account: passwd -u <user>
- Login as service user: sudo -u <service_user> bash
  
- Set hostname: hostnamectl set-hostname <new_hostname>
- Open port (centos 7): sudo firewall-cmd --zone=public --permanent --add-port=8090/tcp && sudo service firewalld restart

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

- Enable passwordless access to vm: 
ssh-copy-id hfarooqui@192.168.86.115 (copies public key from host to authorized_keys on vm)
OR
cat .ssh/id_rsa.pub | ssh root@<slave_ip> 'cat >> .ssh/authorized_keys'
Make sure permission on /home/$USER (vm) is 755 or 700 (else it will still prompt for password)

#### Install
Java (OpenJDK 8): sudo yum install java-1.8.0-openjdk-devel
Enable (Jenkins) repository: curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
Add repository to system: sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

#### Definitions
##### What are the functional differences between .profile and .bash_profile?
.bash_profile and .bashrc are specific to bash, whereas .profile is read by many shells in the absence of their own shell-specific config files.
The idea behind this was that one-time setup was done by .profile (or shell-specific version thereof), and per-shell stuff by .bashrc. For example, you generally only want to load environment variables once per session instead of getting them whacked any time you launch a subshell within a session, whereas you always want your aliases (which aren't propagated automatically like environment variables are).

##### Login or non-login shell?
When you login (type username and password) via console, either sitting at the machine, or remotely via ssh: .bash_profile is executed to configure your shell before the initial command prompt. But, if you’ve already logged into your machine and open a new terminal window (xterm) inside Gnome or KDE, then .bashrc is executed before the window command prompt. .bashrc is also run when you start a new bash instance by typing /bin/bash in a terminal.

##### What is difference between "deployment key" and "ssh key" in bit bucket
Deployment key is used to grant RO access to the repo. Whereas ssh key is used to grant full access to repo

##### sysctl vs systemctl
sysctl and systemctl are two completely different things, and none replaced the other.
sysctl sets or queries certain kernel parameters (see "man sysctl"), while systemctl allows to communicate with systemd.
