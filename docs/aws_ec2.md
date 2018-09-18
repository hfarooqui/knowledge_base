# Elastic Compute Cloud (EC2)

EC2 is a web service that provides resizable compute capacity in the cloud.
It's just a  virtual machine in the cloud and it reduces the time required to obtain and boot new servers to minutes allowing you to quickly scale capacity both up and down as you computing requirements change.

### EC2 options
- **On-Demand**:
  - Allows you to pay fixed rate by hour (or by seconds) with no commitment
  - Perfect for users that want the low cost and flexibility of Amazon EC2 without any upfront payment or long term commitment
  - Application with short term, spiky or unpredictable workloads that cannot be interrupted.
  - Applications being developed or tested on Amazon EC2 for the first time

- **Reserved**:
 - Provides you with a capacity reservation and offers significant discount on the hourly charge for an instance. 1 year or 3 year
 - Applications with steady state or predictable usage
 - Applications that require reserved capacity
 - Users can make upfront payment to reduce their total computing cost even further
  - Standard RI's (upto 75% off on-demand)
  - Convertable RI's (upto 54% off on-demand) features the ability to change the attributes of RI
  - Scheduled RI's are available to launch within the time window you reserve

- **Spot**:
- Allows you to bid whatever price you want for instance capacity providing for even greater savings if your instances have flexible start and end time
 - Applications that have flexible start and end time
 - Applications that are feasible at very low compute prices
 - Users with an urgent need for large amount of additional computing capacity

- **Dedicated Hosts**: 
- Physical EC2 servers decicated for your use. Dedicated hosts can help you reduce cost by allowing you to use your existing server bound software licenses
- Useful for regulatory requirements that do not allow multi-tenancy
- Can be purchesed on-demand (hourly)

### EC2 instance types
![EC2_Types](https://s3.amazonaws.com/hfcontents/kbimages/EC2_Types.png "EC2_Types")
FIGHT DR MC PX

### System Status Check
Checks wheather insatance is reachable
These checks monitor if AWS systems required to use this instance and ensure they are functioning properly
In case of failure reboot/terminate

### Instance Status Check
Makes sure you can get traffic to the operating system
These checks monitor your software and network configuration for this instance
In case of failure reboot the instance so that it gets launched on different host

### Security Group
 - Virtual firewall for your instances
 - One security group can be applied to multiple instances
 - All Inbound traffic is blocked by default
 - All Outbound traffic is allowed
 - Changes to security group take effect immediately
 - You can have any number of EC2 instances within security group
 - You can have multiple EC2 instances attached to security group
 - Security group are **STATEFUL**
   If you create an inbound rule allowing traffic in, that traffic is automatically allowed back out again
 - You cannot block specific IP address using security groups instead use Network Access Control List (STATELESS)
 - You can specify allow rules but not deny rules

# Elastic Block Storage (EBS)

### EBS Volume types
 - **General Purpose SSD (GP2)**
  - General purpose, balances both price and performance
  - Ratio of 3 IOPS/GB with up to 10k IPOS with the ability to burst upto 3k IOPS for extended period of time for volumes at 3334 GiB and above
 - **Provisioned IOPS SSD (IO1)**
  - Designed for IO intensive operations such as large relational or NoSQL DB's
  - Use if needed more than 10k IOPS
  - Provision upto 20k IOPS per volume
 - **Throughput optimized HDD (ST1)**
  - Before SSD we had magnetic storage volume
  - Big data
  - Log processing
  - Data warehousing
  - Cannot be a boot volume
 - **Cold HDD (SC1)**
  - Cannot be a root volume
  - Low cost storage for infrequently accessed workloads
  - File server
 - **Magnetic (Standard)**
  - Lowest cost per gigabyte of all EBS volume types that is bootable
  - These are ideal for workloads where data is accessed infrequently
  - Suitable for applications where the lowest storage cost is important

### Volumes and Snapshots
- Volumes exists on EBS
 - Virtual Hard Disk
- Snapshots exist on S3
- Snapshots are point in time copies of Volumes
- Snapshots are incremental - this means that only the blocks that have changed since the last snapshots are moved to S3
- **Security**
 - EBS root volumes of default AMI's cannot be encrypted
 Snapshots of encrypted volumes are going to be encrypted automatically
 - Volumes restored from encrypted snapshots are encrypted automatically
 - You can share snapshots only if they are encrypted

### Snaphsot of Root Device Volumes
- To take the snapshot of EBS volumes that serve as root device, you should stop the instance before taking the snapshot
- You can create AMI's from EBS backed instances and snapshots
- You can change EBS volume size and type on the fly (best practise is to stop the instance)
- EBS volume will always be in the same AZ as the instance
- To move EBS volume from one AZ to another, take a snaphot or image of it and then copy it to the new AZ/region


# Explore
- SG - All Outbound traffic is allowed vs If you create an inbound rule allowing traffic in, that traffic is automatically allowed back out again
- EBS IOPS
