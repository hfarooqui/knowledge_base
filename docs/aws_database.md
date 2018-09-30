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
