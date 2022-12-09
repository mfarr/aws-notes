## Definitions

* **EC2 (Elastic Compute Cloud)**: Like a VM, hosted in AWS instead of in your own data center
* **Instance Type**: Determines the hardware capabilities of the host computer
* **EBS (Elastic Block Store)**: Storage volumes that can be attached to your EC2 instances

## Pricing Options

* On Demand: Pay by the hour or second depending on the type of instance you run
	* Flexible, short-term, spiky or unpredictable workloads that can't be interrupted, testing the water
* Reserved: Reserved capacity for one or three years. Up to 72% discount on the hourly charge. Regional
	* Predictable usage, specific capacity requirements, pay up-front
	* Standard (no changes to instance type), convertible (can change to greater or equal), scheduled (launch within time window)
* Spot: Purchase unused capacity at a discount of up to 90%. Prices fluctuate with supply and demand. Specify a maximum price, instance is terminated/hibernated when price exceeds maximum
	* Apps that have flexible start/end times, feasible only at low compute prices, urgent need of large amounts of temporary additional capacity
* Dedicated: A physical EC2 server dedicated for your use. The most expensive option.
	* Compliance requirements, software licensing
	* On demand or reserved

* Savings plans
	* Commit to 1 or 3 years of specific compute power measured in $/hour
	* Also includes serverless tech like Lambda and Fargate

* AWS Pricing Calculator

## EBS

[Volume Types Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html)

* (Exam) Understand differences between types and when to use them
* EC2 instance has at least one EBS volume attached for OS
* Highly available - automatically replicated within a single Availability Zone
* Scalable - Dynamically increase capacity and change type with no downtime or performance impact
* General Purpose SSD (gp2, gp3)
	* Balance of price and performance
	* Great for boot volumes, development, test instances, apps that aren't latency sensitive
* Provisioned IOPS SSD (io1, io2)
	* Highest performance volume, most expensive
	* Use if you need more than 16,000 IOPS, I/O intensive applications, latency-sensitive workloads, high durability
* Provisioned IOPS SSD/io2 Block Express - SAN in the cloud (Exam??)
	* Highest performance, sub-millisecond latency
	* Great for the largest, most critical, high-performance apps like DB servers
* Throughput Optimized HDD (st1)
	* Low-cost HDD volume
	* Baseline throughput of 40 MB/s per TB, boost of 250 MB/s per TB
	* Frequently-accessed, throughput-intensive workloads
	* Big data, data warehouses, ETL, log processing
	* Cost effective way to store large amounts of frequently-accessed data
	* Cannot be a boot volume
* Cold HDD (sc1)
	* Lowest cost option
	* Baseline throughput of 12 MB/s per TB, boost of 80 MB/s per TB
	* Good for apps that need the lowest cost and performance is not a factor
	* Cannot be a boot volume
* IOPS vs Throughput
	* IOPS - measures the number of r/w ops per second
		* The ability to action reads and writes very quickly
	* Throughput - measures the number of bits r/w per second (MB/s)
		* Metric for large datasets, complex queries
		* The ability to deal with large datasets
* Exam tips
	* EBS - Highly available and scalable storage volumes that you can attach to an EC2 instance
		* gp2 - General purpose SSD, boot volumes, general app usage
		* gp3 - Latest generation general purpose SSH, 20% cheaper than gp2
		* io1 - Suitable for online transaction processing, latency-sensitive volumes. Up to 64,000 IOPS per volume. Most expensive, high performance, high durability. 64,000 IOPS per volume.
		* io2 - Latest generation, greater number of IOPS per GiB, higher durability than io1. Same price as io2. 64,000 IOPS per volume.
		* io2 Block Express - Largest, most critical, high performance applications. 256,000 IOPS per volume. SAN in the cloud!
		* st1 - Throughput optimized HDD. Suitable for Big Data, data warehouse
		* sc1 - Cold HDD. Less frequently accessed data, performance is not an issue. Lowest cost.
	* Default Encryption - If encryption by default is set on account by admin, you can't create unencrypted EBS volumes
	* Encrypted Snapshots - If you create an EBS volume from an encrypted snapshot, the volume will also be encrypted
	* Unencrypted Snapshots - If you create an EBS volume from an unencrypted snapshot, encryption on the volume is optional if not set as required at account level.

## Elastic Load Balancer

* Load balancer - distributes network traffic across a group of servers
* Easily increase capacity when needed
* Types
	* Application load balancer - HTTP and HTTPS
	* Network load balancer - TCP and high performance
	* Classic (Legacy) load balancer - HTTP/HTTPS and TCP
	* Gateway Load Balancer (Exam??) - load balance workloads for thir-party virtual appliances running in AWS
* Application
	* Operate at Layer 7 and are application aware
	* Supports advanced request routing to specific web servers based on the HTTP header
* Network
	* High performance option. Used for load balancing TCP traffic where extreme performance is required
	* Operates at Layer 4
	* Most expensive option
* Classic
	* Legacy
	* Supports some Layer 7-specific features such as X-Forwarded-For headers and sticky sessions
	* Support Layer 4 load balancing for applications that rely purely on TCP protocol
* X-Forwarded-For Header
	* Identify the originating IP address of a client connecting through a load balancer
* Common Load Balancer errors
	* Error 504 Gateway Timeout - check application. Destination server has timed out for some reason.
* Exam Tips
	* Application Load Balancer
	* Network Load Balancer
	* Classic Load Balancer
	* Gateway Load Balancer
	* X-Forwarded-For

## Route 53

* DNS
* Hosted Zone - group of DNS records for domain
* Created an Alias to our Application Load Balancer

## AWS CLI

* Exam tips
	* Principle of Least Privilege
	* Use IAM groups and assign users to groups
	* Group permissions are assigned using IAM policy documents
	* Secret Access Key - will only see this one. If lost, you will need to delete and recreate access key
	* Don't share key pairs!
	* CLI supports Linux, Windows, macOS
	* Use pagination to limit/expand results (default 1000)
	* Roles are preferred from a security perspective
	* Avoid hard-coding credentials in application or instances
	* Policies are used to control a role's permissions
	* Policy updates take immediate effect
	* Attaching/detaching roles takes immediate effect

## RDS

* When? Generally used for online transaction processing (OLTP)
* MySQL, SQL Server, Amazon Aurora, PostgreSQL, etc.
* Online Transaction Processing (OLTP) vs Online Analytics Processing (OLAP)
	* OLTP - Processes data from transactions in real-time. Data processing and completing large numbers of small transactions in real-time
	* OLAP - Processes complex queries to analyze historical data. Data analysis using large amounts of data and complex queries that take a long time to complete.
* RDS is not suitable for OLAP workloads. Use RedShift instead.
* Exam tips
	* Number of choices of different RDS systems
	* OLTP workloads
* Multi-AZ
	* Exact copy of your prod database in another Availability Zone. AWS handles replication.
	* Basically all DB types can be used in Multi-AZ
	* For DR, not improving performance. Can't connect to primary and standby simultaneously
	* Automatic failover to standby instance.
* Read Replica
	* A read-only copy of your primary database (eg. BI server)
	* Increase or scale read performance.
	* Great for read-heavy workloads.
	* Can be cross-AZ or even cross-region.
	* Has its own DNS endpoint different than primary DB.
	* Can be promoted to their own DB, but breaks replication.
	* For scaling, not DR!
* Backups and Snapshots
	* Snapshots
		* Manual, ad-hoc, user-initiated snapshot of storage volume attached to DB instance.
		* No retention period.
		* Backup to a Known State
		* Point-in-time snapshot only.
	* Automated Backup
		* Enabled by default, daily backups or snapshots that run during backup window. Transaction logs are used to replay transactions.
		* Point-in-Time Recovery - any point in time within defined retention period.
		* Retention period - between 1 - 35 days.
		* Recovery - Selects most recent daily backup and then applies transaction logs to recovery point.
		* Stored in S3, free storage space equal to size of database.
		* Storage IO may be suspended for a few seconds during backup window when backup initializes
	* Restored version will always be a new RDS instance with a new DNS endpoint.
* Encryption at Rest
	* Uses AWS KMS
	* Includes all underlying storage, automated backups, snapshots, logs, read replicas.
	* Must be enabled at DB instance creation time
		* Or: create snapshot, encrypt snapshot, restore to new encrypted DB

## ElastiCache

* In-Memory Cache (Key Value)
* Improves database performance
	* Retrieve from fast memory cache instead of slow disk-based storage
* Great for read-heavy database workloads
	* Caching the results of IO intensive database queries, storing session data for distributed applications
* Two types
	* Memcached
		* Basic object caching, simplest choice
		* Scales horizontally but no persistence, Multi-AZ or failover
	* Redis
		* More sophisticated with enterprise features like persistence, replication, Multi-AZ and failover
		* Supports sorting and ranking data and complex data types
* Good choice if database is particularly read-heavy and not prone to frequent changing
* Won't help with heavy write loads, OLAP queries

## Systems Manager Parameter Store

* Store parameters used by applications like license keys, database connection strings, user names, passwords, etc.
* Can be passed in to Lambda, CloudFormation, EC2, other AWS services
* Plaintext or encrypted using KMS
