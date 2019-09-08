## Linux Distributions
3 main distributions
#### Debian based
  - The grand daddy. Supported by largest community
  - Very stable
  - Use debian vanilla in production
  - For Desktop envionments use Ubuntu, LinuxMint(Not very secure, was breached Feb 2016, popular among non-technical users)
#### REHL based
  - Managed by RedHat enterprise which provides support. Useful for business critical environments.
  - Very reliable and secure
  - Use lightwight CentOS in production
  - For Desktop environments use Fedora
  - Little behind Debian community in terms of package availability
#### Arch based
  - Run by the so called elites
  - Had latest and greatest packages
  - Not very stable if using 'AUR' (builds program for you with single command line). Uses "pacman" package manager
  - Use Manjaro to get your hands dirty

Other distrbutions include Gentu, ClearOS(Intel)
## Shell Commands

#### User
- Create user: <b> usermod $USERNAME </b>
- Set Password: <b> passwd $USERNAME </b>
- Lock an account: <b> passwd -l $USERNAME </b>
- Unlock an account: <b> passwd -u $USERNAME </b>
- Login as service user: <b> sudo -u $SERVICE_USER bash </b>
- Add user to group: <b> sudo usermod -a -G $GROUPNAME $USERNAME </b>
- Set hostname: <b> hostnamectl set-hostname $NEW_HOSTNAME </b>


- Open port (centos 7): <b> sudo firewall-cmd --zone=public --permanent --add-port=8090/tcp && sudo service firewalld restart </b>

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

- Enable passwordless access to vm:  </br>
<b>ssh-copy-id hfarooqui@192.168.86.115</b> (copies public key from host to authorized_keys on vm) </br>
OR </br>
<b>cat .ssh/id_rsa.pub | ssh root@<slave_ip> 'cat >> .ssh/authorized_keys'</b> </br>
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
