##### ***Self-hosted MongoDB backup***
###### ***What is Elastic BlockStore (EBS)?***
Elastic Block Store (EBS) is a scalable block storage service provided by Amazon Web Services (AWS). It's designed to be used with Amazon EC2 instances for both throughput and transaction-intensive workloads at any scale. An EBS volume behaves like a raw, unformatted, external block device that you can attach to a single EC2 instance. EBS volumes are particularly useful for database storage, file systems, or any application that requires fine-tuned block-level storage.
###### ***Backup Strategies Explained***
**Hot Standby Replica on EBS**
Here, a replica instance is always kept ready on an EBS volume. This is known as a "hot standby" because the replica is ready to take over immediately in case the primary instance fails. The reason for using EBS instead of NVMe/Instance Store is for data persistence and reliability. EBS volumes are not tied to the lifetime of an instance, and the data persists even if the instance is terminated.

**How to Accomplish:**
1. **Create an EBS Volume**: You can do this from the AWS Management Console.
2. **Attach EBS to EC2 Instance**: Attach the created EBS volume to the replica MongoDB instance.
3. **Sync Data**: Ensure the data is in sync between your primary MongoDB and the replica on EBS.
4. **Monitor**: Make sure to continuously monitor the replica for any discrepancies or issues.
###### ***Backup Using `mongodump`, Compression, and Upload to S3***
In this method, a `mongodump` action is executed on a replica to create a dump of the database. The dump is then archived and compressed using `zip`. The zip file is split into smaller chunks and uploaded directly to an S3 bucket.
**How to Accomplish:**
1. **Run `mongodump`**: Execute the `mongodump` command on the MongoDB replica to create a dump of the database.
    `mongodump --host replica_host --port replica_port --out /path/to/dump`
2. **Archiving and Compression**: Compress and archive the dumped data. The `zip` command can also split the zip into smaller parts.
    `zip -r -s 1G archive.zip /path/to/dump/`
    
    Here, `-s 1G` will split the zip into 1GB chunks.
3. **Upload to S3**: Use AWS CLI or SDK to upload the compressed chunks to an S3 bucket.
    `aws s3 cp archive.zip* s3://your-s3-bucket/path/`
######  ***Summary***
- **EBS** provides a block-level storage solution that is scalable and reliable, ideal for database systems like MongoDB.
- **Hot Standby on EBS**: Keeps a MongoDB replica synced and ready on an EBS volume for immediate takeover.
- **Backup and Upload to S3**: Using `mongodump` for creating database dumps, compressing them, splitting them into manageable sizes, and then uploading them to S3 ensures that you have a scalable and efficient backup system.

Both strategies aim to provide a robust backup solution that is both reliable and efficient, essential for managing trillion+ document databases.