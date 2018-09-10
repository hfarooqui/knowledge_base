#Simple Storage Service (S3)

###The Basics
- Secure, durable and highly Object based storage
- Data is spread across multiple devices and facilities
- Allows you to upload files ranging from 0 bytes to 5 TB
- Unlimited storage but you pay for usage per GB
- Files ares stored in buckets (Folder in cloud)
- S3 is universal namespace i.e. name must be unique globally. 
E.g. https://s3-eu-west-1.amazonaws.com/hfarooquidocs
- You will receive HTTP 200 if file upload to S3 bucket was successful
- 99.99% availability
- 99.99999999999% (11 9's) durability (cannot lose file)
- Tiered storage
 - S3 Standard: 99.99% availability, 99.99999999999% durability, stored reduntantly across multiple devices in multiple facilities, designed to sustain loss of 2 facilities concurrently. No retrival fee but most expsnsive.
  - S3 IA (Infrequently accessed): For data that is accessed less frequently but require rapid access when needed. Cheaper compared to "S3 Standard" but you are charged a retrival fee
  - S3 One Zone IA: Data stored only in one AZ. Cheaper than above tow tiers
  - Glacier: Cheapest but used for data archival only. Expedidated, Standard or Bulk. Standard retrival takes around 3-5 hours
- Charges are for:
	- Storage
	- Retrival/Requests
	- Storage Management Pricing
	- Data Transfer Pricing
		- Transfer Acceleration: Enables fast, easy and secure transfers of files over long distances between end users and S3 bucket. It takes advantage of Amazons CloudFront's globally distributed Edge locations (much closer to end user compared to S3 location). As the data arrives at Edge location it is routed to Amazons S3 over optimized network path
- Lifecycle management (E.g. Delete files older than 30 days or move it from one storage tier to another)
- Encryption
 - Client side encryption
 - Server side encryption
  - with Amazon S3 Managed Keys (SSE-S3)
  - With KMS (SSe-KMS)
  - With customer provided keys (SSE-C)
- By default buckets are private and all the files stored inside the bucket are private

###Data consistancy model for S3
- Read after write consistancy for PUTS of new objects
(File is available for read immediately after it is uploaded)
- Eventual consistsncy for overwrite PUTS and DELETS (When you update or delete the existing file you might still get the old copy if you attempt to read it. The reason being it take few seconds to update/delete file to different AZ's)

###S3 Components
- Key: Name of the object (file)
- Value: Data inside the file
- Version Id: File version
- Metadata: Data about data. (Tags)
- Subresources:
 - ACL: Define permisssions for files in the bucket
 - S3 bucket policies: Applied at bucket level
 - Torrent: Useful for torrenting?
