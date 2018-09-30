# AWS Databases

## Types of supported databases
![AWS_Databases](https://s3.amazonaws.com/hfcontents/kbimages/AWS_databases.png "AWS_Databases")


## Backups
- **Automated Backups**
    - Allows you to restore your database to any point in time within a retention period
	- Retention period can be between 1 -35 days
	- Automated Backups takes full daily snapshots and also stored transactional logs throughout the day
	- When you do a restore AWS will first choose the most recent backup and then apply transactional logs relevant to that day. This allows you to do point in time recovery down to second within the retention period
	- Enabled by default, Snaphots stored on S3 (you get free amount of storage on S3 as the size of your DB)

- **Snapshots**
   - DB Snapshots are done manually
   - They are stored even if you delete the original RDS instance, unlike automated backups

## Restore
 - Whenever you restore a DB (Automated or manual), the restored version of the database will be the new RDS instance with new DNS endpoint

## Encryption
 - Encryption is supported for - MySQL, Oracle, SQL-Server, Postgres, MariaDB and Aurora
 - Encryption is done using AWS KMS
 - Once RDS instance is encrypted, data stored at rest in the urderlying storage is encrypted, as are its automated backups, read replicas and snapshots
 - Encrypting existing DB instance is not supported
 - To encrypt the existing DB, you must first create a snapshot, make a copy of it and encrypt the copy

## Multi-AZ RDS
![Multi-AZ_RDS](https://s3.amazonaws.com/hfcontents/kbimages/Multi-AZ_RDS.png "Multi-AZ_RDS")
- Multi-AZ allows you to have exact copy of production DB in different AZ
- AWS handles replication, which means when data is written to Production, this write will automatically be synchronised to standby DB
- In an event of planned DB mantainnce, DB instance failure or AZ failure, RDS will automatically failover to standby so that DB operations can resume quickly without administrative intervention
- Multi-AZ is for DR only. It is not primarily used for performance improvement (For performance improvement you need to use read replica)

## Read Replica
![RDS_Read_Replica](https://s3.amazonaws.com/hfcontents/kbimages/RDS_ReadReplica.png "RDS_Read_Replica")
- Read replica allow you to have readonly copy of your production database
- This is achived by using async replication from primary DB to read replica
- Read replicas are used for very read-heavy workloads (Reporting)
- Primarily use for scaling (performance improvements) not DR
- Not available for SQL_Server or Oracle
- Must have automatic backup truned on in order to deploy read replica
- Can have 5 copies of Read replicas for any DB
- Each read replica will have its own DNS
- You can have read replicas that can have multi-az
- You can create read replicas of Multi-AZ source DB's
- Read replicas can be promoted to their own DB's, this can however break the replication
- You can have read replica in second region
- You can apply encryption on Read Replica even though Primary DB is not encrypted

## Dynamo DB
- Fast and Flexible NoSQL DB service for all applications that need consistant, millisecond latency at any scale
- Used when fast read access is desired
- Cheaper reads, expensive writes
- Fully managed DB that supports both document and key value data models
- Flexible data model and reliable performance makes it a great fir for mobile, web, gaming IoT and amy other applications
- Stored on SSD storage
- Stored across 3 geographically distinct data centers
- Eventual Consistant Reads
  - Consistancy across all copies of data is reached within a second. Repeating a read after a short time should return the updated data (Best read performance)
- Strong Consistncy Reads
  - A strongly consistant read returns a result that reflects all writes that received a successful response prior to the read
- Item is a row in a table
- Can dynamically add columns since its a NoSQL DB (Each row can have additional one or more columns)
- Pricing
  - Provision Throughput capacity
     - Write Throughput 0.0065 per hour for every 10 units (1 Unit = #of writes per second)
	 - Read Throughput 0.0065 per hour for every 50 units (1 Unit = #of reads per second)
 - Storage cost up to $0.25/GB


## RedShift
- Fast, Fully managed, Peta byte scale Data Warehouse service in the cloud
- Starts with $0.25/hr to $1000/TB/yr
- Cheaper compared to other OLAP/Data warehousing solutions
- Data Warehousing database use different type of architecture both from DB prespective and infrastructure layer
- **Single node** (160GB)
- **Multi node**
	- Leader node: Manages client connections and receives queries
	- Compute node: Stores data and perform queries and computation. Upto 128 compute nodes
- **Columner Data Storage**: Instead of storing data as series of rows, RedShift orgnizes data by column. Row based systems are ideal for OLTP whereas column based systems are ideal for data warehousing and analytics where queries often involve aggregates performed over large datasets. Column based systems require fewer I/Os, greatly improving query performance
- **Advanced Compression**: Columner data stores can be compressed much more than rowbased data stores because similar data is stored sequentially on disk.
- Doesn't require indexing or views so use less space than treditional RDBMS
- When loading data into empty table, Redshift automatically samples data and selects most appropriate compression scheme
- **Massively Parallel Processing**: Redshift automatically distributes data and query load across all the nodes. Makes it easy to add new nodes and enables you to maintain fast query performance as your data warehouse grows
- **Pricing**: 
   - Compute Node Hours: Total number of hours you will run across all the compte nodes for the billing period. you are billed one unit per node, per hour hence a 3-node cluster running persistantly will incur 2160 instance hours
   - Backup
   - Data transfer
- **Security**
   - Encrypted in-transit using SSL
   - Encrypted at rest using AES-256 encryption
   - By default RedShift takes care of key management
     - Manage your own keys using Hardware Security Module (HSM)
	 - AWS KMS
- **Availability**: 
    - Currently available in only 1 AZ
	- Can restore snapshot to new AZ in an event of outage

## Elasticcache
- Web service that makes it easy to deploy, operate and scale in-memory cache in the cloud
- Service improves performance of web application by allowing you to retrive information from fast, managed, in-memory caches instead of relying entirely on slower disk based databases
- Used to improve latency and throughput for many read heavy application workloads or compute intensive workloads
- Good choice if your DB is read heavy not prone to frequent changes

### Types of Elasticache
- **Memcache**
    - Widely adopted memory object caching system
	- Elasticache is protocol compliant with Memcache, so popular tools that you use today with Memcache environment will work seamlessely with the service
- **Redis**
   - Populare open source in-memory key-value store that supports datastructure such as sorted sets and lists.
   - Supports Master/Slave replication and Multi-AZ which can be used to achive cross AZ redundancy
