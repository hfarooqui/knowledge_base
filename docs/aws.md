# AWS:

### Links:
https://acloud.guru/ for questions
https://aws.amazon.com/free/
https://aws.amazon.com/certification/certified-solutions-architect-associate/

------------

### Block storage
Traditional block storage device — like a hard drive — over the network

Cloud providers often have products (volumes) that can provision a block storage device of any size and attach it to your cloud instance.

You can run your own using OpenStack Cinder, Ceph, or the built-in iSCSI service available on many NAS devices

#### Advantages:

1. You can easily take live **snapshots** of the entire device for backup purposes
2. Block storage devices can be **resized** to accommodate growing needs
3. You can easily **detach and move** block storage devices between machines
4. Block storage devices provide **low latency IO**, so they are suitable for use by databases.
5. Block devices are **well supported**. Every programming language can easily read and write files
6. Well suited for storing data in **traditional databases**. Additionally, many legacy applications that require normal **filesystem storage** will need to use a block storage device.

#### Disadvantages:

1. Storage is **tied to one server** at a time
2. Blocks and filesystems have **limited metadata** about the blobs of information they're storing (creation date, owner, size). Any additional information about what you're storing will have to be handled at the application and database level, which is additional complexity for a developer to worry about
3. You need to **pay for all the block storage** space you've allocated, even if you're not using it
4. You can only access block storage through a **running server**
5. Block storage needs **more hands-on work and setup** vs object storage (filesystem choices, permissions, versioning, backups, etc.)


------------

### Object storage
Object storage is the storage and retrieval of unstructured blobs of data and metadata using an HTTP API.  Instead of breaking files down into blocks to store it on disk using a filesystem, we deal with whole objects stored over the network.

These objects could be an image file, logs, HTML files, or any self-contained blob of bytes. They are unstructured because there is no specific schema or format they need to follow.

Object storage is useful for hosting static assets, saving user-generated content such as images and movies, storing backup files, and storing logs

There are some self-hosted object storage solutions, though you will give up some of the benefits of a hosted solution (such as not having to worry about hard drives and scaling issues). You could try Minio, a popular object storage server written in the Go language, or Ceph, or OpenStack Swift.

#### Advantages:

1. A simple **HTTP API**, with clients available for all major operating systems and programming languages
2. Object storage services charge only for the storage space you use
3. Some object stores offer **built-in CDN integration**, which cache your assets around the globe to make downloads and page loads faster for your users
4. Optional **versioning** means you can retrieve old versions of objects to recover from accidental overwrites of data
5. Object storage services can easily **scale** from modest needs to really intense use-cases without the developer having to launch more resources or rearchitect to handle the load
6. Using an object storage service means you **don't have to maintain hard drives and RAID arrays**, as that's handled by the service provider
7. Being able to store chunks of metadata alongside your data blob can further **simplify your application architecture**


#### Disadvantages:

1. You can't use object storage services to back a traditional database, due to the **high latency** of such services
2. Object storage **doesn't allow you to alter just a piece of a data blob**, you must read and write an entire object at once. This has some performance implications. For instance, on a filesystem, you can easily append a single line to the end of a log file.
On an object storage system, you'd need to retrieve the object, add the new line, and write the entire object back. This makes object storage less ideal for data that changes very frequently
3. Operating systems **can't easily mount an object store like a normal disk**. There are some clients and adapters to help with this, but in general, using and browsing an object store is not as simple as flipping through directories in a file browser
