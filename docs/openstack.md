Node Provisioning (Nova Baremetal)﻿

Deployment and user images are prepared and loaded to glance 
     Deploy Images (Network Boot Program)
deploy—vmlinuz-ramdisk
deploy-initrd-kernel

    User Images 
initrd-ramdisk (for hlinix)
vmlinuz-kernel (for hlinux)
hlinux os image (user image for hlinux)

BM database is created
System Admin/Cloud Admin enrolls the HW with baremetal driver running on Compute\BM Hosts
This the registration or the enrollment process. At this point the enrolled hardware details
from baremetal.csv are stored in nova_db database


Once the hardware is enrolled with baremetal driver, the Nova compute process will broadcast the availability of a new compute resource to the Nova scheduler during the next periodic update, which by default occurs once a minute. After that, enrolled hardware will be provisioned upon request.
At this point the registered node should appear in hypervisor list
The baremetal driver on nova-compute selects the hardware matching enrolled baremetal node from its pool, based on hardware resources and the qualifier type (# of cpus, memory, HDDs).


nova-compute\host configuration (/etc/nova/nova.conf)

    [baremetal] 
    sql_connection=mysql://$ID:$Password@$IP/nova_bm
    compute_driver=nova.virt.baremetal.driver.BareMetalDriver
    baremetal_driver=nova.virt.baremetal.pxe.PXE
    power_manager=nova.virt.baremetal.ipmi.Ipmi
    instance_type_extra_specs=cpu_arch:x86_64
    baremetal_tftp_root = /tftpboot
    scheduler_host_manager=nova.scheduler.baremetal_host_manager.BaremetalHostManager
    net_config_template = /opt/stack/nova/virt/baremetal/net-static.ubuntu.template
    baremetal_deploy_kernel = xxxxxxxxxx
    baremetal_deploy_ramdisk = yyyyyyyy 

vegas lab
#BAREMETAL

my_ip=10.10.44.16

compute_driver=nova.virt.baremetal.driver.BareMetalDriver
firewall_driver=nova.virt.firewall.NoopFirewallDriver
scheduler_host_manager=nova.scheduler.baremetal_host_manager.BaremetalHostManager

ram_allocation_ratio = 1.0
reserved_host_memory_mb = 0

dhcp_options_enabled=true


[baremetal]
sql_connection=postgresql://nova_bm:password@192.168.198.10/nova_bm
driver=nova.virt.baremetal.pxe.PXE
power_manager=nova.virt.baremetal.ipmi.IPMI

instance_type_extra_specs=cpu_arch:x86_64
tftp_root=/tftpboot
terminal=/usr/local/bin/shellinaboxd
pxe_bootfile_name=pxelinux.0


baremetal_type=baremetal


type=baremetal

pxe_interface=eth3


Baremetal-deploy-helper powers on the baremetal node thorough IPMI (Intelligent Platform Management Interface)

Baremetal-deploy-helper PXE boots "target node" with deploy-ramdisk + kernel
Ones the node is powered on DHCP component of the node’s PXE firmware broadcasts a DHCPDISCOVER packet containing PXE-specific options to port 67 (DHCP server port). It asks for the required network configuration and network booting parameters. 
The PXE-specific options identify the initiated DHCP transaction as a PXE transaction
DHCP server answers with a regular DHCPOFFER carrying networking information (i.e. IP address) and PXE specific parameters. (Provided that DHCP server is PXE enabled. Non PXE enabled server will be able to answer with a regular DHCPOFFER carrying networking information (i.e. IP address) but not the PXE specific parameters. However, node will not be able to boot if it only receives an answer from a non PXE enabled DHCP server without PXE specific parameters
After parsing a PXE enabled DHCP server (DHCPOFFER), the node will be able to set its own network IP address, IP Mask and point to the network containing booting resources, based on the received TFTP Server IP address and the name of the Network Booting Program (bootstrap). 
The node transfers the deploy images into its own RAM using TFTP, possibly verifies it, and finally boots from it. 
(Network boot program are just the first link in the boot chain process and they generally request via TFTP a small set of complementary files in order to get running a minimalistic OS executive (deploy-ramdisk))
deploy ram disks initrd facility creates temporary file system  containing the SCSI module and then loads the modules   
deploy-ramdisk calls back to nova's baremetal helper to notify that iscsi is ready (port 3260)
Ones the signal is received Baremetal-deploy-helper 
node status is changes to  "DEPLOYING"
Logs in baremetal node's local disks via iSCSI (connects to iSCSI end point)
partitions volumes (fdisk),
dd's the updated glance image onto it (baremetal-deploy-helper signals BM node on Port 10,000 ones the transfer is complete) 

            When the small OS (deploy ramdisk+kernel) is alive it loads its own fully capable network drivers, a full TCP/IP stack, and the rest of transfers for booting or installing a full OS (user image) are                   performed not by TFTP but at this point using more robust transfer protocols like iSCSI, HTTP, CIFS, NFS, etc  

Baremetal-deploy-helper PXE boot target node with initrd-ramdisk+ vmlinux -kernel from the (User) image 
kernel and ramdisk images which are used by the compute host to write the user-specified image onto the baremetal node.

Reboots node (PXE boot)
Baremetal-deploy-helper changes node state in DB to "DEPLOYDONE"
PXE driver notices the state change, does final task and finishes
