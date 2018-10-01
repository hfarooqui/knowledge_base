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

### VPC Peering
- Allows you to connect one VPC to another via a direct network route using private IP address
- Instances behave as if they were in the same private network
- You can peer VPC with other AWS account as well as with other VPC's in the same account
- Peering is always in Start Configuration (Hub and Spoke model) where you have 1 Central VPC with 4 other VPC's. There is not TRANSITIVE PEERING (If A can talk to B and A can talk to C, this doesn't me B can talk to C)
![VPC_Peering](https://s3.amazonaws.com/hfcontents/kbimages/VPC_Peering.png "VPC_Peering")
