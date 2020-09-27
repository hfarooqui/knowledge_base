## Caching
https://d0.awsstatic.com/whitepapers/Database/database-caching-strategies-using-redis.pdf

## Clustering
- The process of organizing objects into groups whose members are similar in some way"

## Memcache vs Redis
- Both as key/value datastore
- Redis is more more "data structure store". Redis supports more datatypes as compard to memcache (strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams)
- Memcached’s internal memory management,is more efficient in the simplest use cases because it consumes comparatively less memory resources for metadata. Strings (the only data type supported by Memcached) are ideal for storing data that is only read, because strings require no further processing
- Redis supports 6 eviction policies, whereas memcache only supports LRU policy
- Memcached could be preferable when caching relatively small and static data, such as HTML code fragments
- Memcached is multithreaded, you can easily scale (verticle scaling) up by giving it more computational resources, but you will lose part or all of the cached data (depending on whether you use consistent hashing)
- Redis is mostly single-threaded and can scale horizontally via clustering without loss of data. Clustering is an effective scaling solution, but it is comparatively more complex to set up and operate

# Cloud-init
Cloud-init is the industry standard multi-distribution method for cross-platform cloud instance initialization. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations.

Cloud instances are initialized from a disk image and instance data:

Cloud metadata
User data (optional)
Vendor data (optional)
Cloud-init will identify the cloud it is running on during boot, read any provided metadata from the cloud and initialize the system accordingly. This may involve setting up the network and storage devices to configuring SSH access key and many other aspects of a system. Later on the cloud-init will also parse and process any optional user or vendor data that was passed to the instance.


## Service Mesh
### Service Mesh vs API Gateway

## Consistent hashing

## Architecture considerations
- Tools - Infrastructure management, Configuration management
- Managed vs Hosted Services
- Containers vs Serveless

- Security
- Performance
- Availability (HA)
- Cost - Capacity planing
- Deployment - CI/CD
- Observability Monitoring
- Logging