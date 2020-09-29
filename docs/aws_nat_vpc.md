# Virtual Private Cloud (VPC)

- Virtual Data center in cloud
- Lets you provision logically isolated section of AWS clould where you can launch AWS resources in a virtual network that you define
- You have complete control over networking environment including selection of IP ranges, subnets, route tables and network gateways
- You can easily customize the network configuration of your VPC. For instance you can create public facing subnet for your webservers/load balancers that has access to internet and place your backend systems like DB or application servers in a private facing subnet with no internet access. You can leverage multiple layers of security using Security groups and Network access control list to help control access to EC2 instances in each subnet
- You can create hardware VPN connection between your corporate  datacenter and your VPC and leverage AWS cloud as extenstion of your corporate data center
- All the subnets in the default VPC has route out to the internet
- Security Groups are Stateful (For instance if you allow Inbound traffic on port 80, Outbond bound traffic on port 80 is automatically applied) where as Network Access Control List are Stateless
- 1 Subnet = 1 AZ
![VPC](https://s3.amazonaws.com/hfcontents/kbimages/VPC.png "VPC")

- Create VPC > VPC+RouteTable(Main/Default)+NACL+SG
- Create Subnet
	 - Set Auto-Assign Public IP to Yes for Internet facing subnet
- Create Internet Gateway and attach it to VPC (Only 1 IGW per VPC)
- Override default route table by creating External (default) and Internal Route Tables
	- Assign IGW to External/Default Route Table
![VPC2](https://s3.amazonaws.com/hfcontents/kbimages/VPC2.png "VPC2")
The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use, and cannot be assigned to an instance. For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved:
	- 10.0.0.0: Network address.
	- 10.0.0.1: Reserved by AWS for the VPC router.
	- 10.0.0.2: Reserved by AWS. The IP address of the DNS server is always the base of the VPC network range plus two; however, we also reserve the base of each subnet range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. For more information, see Amazon DNS Server.
	- 10.0.0.3: Reserved by AWS for future use.
	- 10.0.0.255: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address.

### VPC Peering
- Allows you to connect one VPC to another via a direct network route using private IP address
- Instances behave as if they were in the same private network
- You can peer VPC with other AWS account as well as with other VPC's in the same account
- VPC peering cannot be done across regions
- VPC peering cannot be done between VPC's that have overlapping CIDR blocks
- Peering is always in Start Configuration (Hub and Spoke model) where you have 1 Central VPC with 4 other VPC's. There is not TRANSITIVE PEERING (If A can talk to B and A can talk to C, this doesn't me B can talk to C)
![VPC_Peering](https://s3.amazonaws.com/hfcontents/kbimages/VPC_Peering.png "VPC_Peering")

### NAT Gateway
- Prefered by enterprise
- Scale automatically up to 10Gbps
- No need to patch
- Not associated with Security group
- Automatically assigned a public IP address
- Requires to update route tables
- No need to disable source/destination checks
- More secure than NAT instances
- NAT Gateway needs to be added to Route Table which is created by default when you create a VPC
![NAT_Gateway](https://s3.amazonaws.com/hfcontents/kbimages/NAT_Gateway.png "NAT_Gateway")

### NAT Instances
 - Obsolete
 - When creating NAT instances always disable Source/Destination check on the instance
 - NAT instances must be in public subnet
 - There must be a route out of private subnet to NAT instance
 - The amount of traffic that NAT instances support depends on size of the instance
 - You can create HA using Autoscaling groups, multiple subnets in different AZ and script to automate failover
 - Behind the security group

### Network ACL's (NACL)
- Your VPC comes with a default NACL and by default it allows all inbound and outbound traffic
- You can create custom NACL's and by default it denies all the inbound and outbound traffic
- Each subnet in your VPC must be associated with one NACL
- You can associate a NACL with multiple subnets, however you can associate only one NACL to a subnet. When you associate NACL with subnet, previous association is removed
- NACL contains numbered list of rules that is evaluated in order starting with lowest numbered rule
- NACL have seperate inbound and outbound rules
- NACL are stateless; responses to allowed inbound traffic are subject to rules defined for outbound traffic
- Ephimeral ports, outbound rules only on network ACL
- Block IP addresses using NACL and not security group
- If creating internet facing load balancers all the subnets should have Internet Gateway attached to it

### Flow Logs

 - Flow Logs enable you to capture information about IP traffic going to and from network interfaces in your VPC
 - Flow log data is stored using Amazon CloudWatch logs
 - After you have created Flow logs you can view and retrive its data in CloudWatch logs
- Flow Logs data can be captured at 3 levels
  - VPC
  - Subnet
  - Network Interface level
- You can stream logs to Lambda, Elastic Search Service or export data to S3
- You can enable Flow Logs to peered VPC only if it is in the same account
- After you have created Flow Logs, you cannot change its configuration. For example you cannot associate a different IAM role to flow logs
- Not all traffic is monitored:
	- Traffic generated by instances when they contact Amazon DNS server
	- Traffic generated by Windows instance for WIndows License activation
	- Traffic to and from 169.254.169.254 for instance metadata
	- DHCP traffic
	- Traffic to reversed IP address for the default VPC router

### NAT vs Bastion
![NATvsBastion](https://s3.amazonaws.com/hfcontents/kbimages/NATvsBastion.png "NATvsBastion")
- Bastion host (Jump host) is used to securely administrator EC2 instance in private subnet
- NAT Gateway/Instance is used to provide internet traffic to EC2 instances (using SSH or RDP) in private subnet

### VPC Endpoint
- Typically to access any service from Private SN you would have to go through NAT Gateway.
![VPC_Endpoint](https://s3.amazonaws.com/hfcontents/kbimages/VPC_Endpoint.png "VPC_Endpoint")
- A VPC endpoint allows you to securely connect your VPC to another service.
- An interface endpoint is powered by PrivateLink, and uses an elastic network interface (ENI) as an entry point for traffic destined to the service.
- A gateway endpoint serves as a target for a route in your route table for traffic destined for the service.
