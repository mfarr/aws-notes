
## Introduction to DynamoDB

* Fast and flexible NoSQL database
* Supports key-value data models and document formats (JSON, XML, HTML)
* Two consistency models
	* Eventually Consistent Reads (default)
		* Consistency across all copies of data is usually reached within a second
		* Best for read performance
	* Strongly Consistent Reads
		* Always reflects all successful writes, writes are reflected across all 3 locations at once
		* Best for read consistency
	* ACID Transactions (DynamoDB Transactions)
		* Provides the ability to perform Atomic, Consistent, Isolated, Durable  transactions
* Primary Keys
	* Partition Key
		* A unique attribute (eg. customer ID, email address, etc.)
		* Value of the partition key is input to an internal hash function that determines the partition or physical location on which the data is stored
	* Composite Key (partition key + sort key)
		* Use when partition key is not necessarily unique
		* Eg. user_id + timestamp
		* Items may have the same partition key but must have a different sort key

## DynamoDB Access Control

* IAM
	* Create IAM users within your AWS account with specific permissions to access and create tables
* IAM Roles to enable temporary access to DynamoDB
* Restricting User Access
	* Use a special IAM condition to restrict user access to only their own records
* Fine-grained access control using IAM
	* Condition parameter: `dynamodb:LeadingKeys` allows users to access only the items where the partition key value matches their User_ID

## Indexes Deepdive

* Secondary Index
	* Flexible querying based on attribute that is not the primary key
	* Global secondary indexes and local secondary indexes
* Local Secondary Index
	* **Same partition key as original table but a different sort key**
	* **Must be added when creating the table**
		* Can't be added, removed or modified later on
	* A different view of your data
* Global Secondary Index
	* **Different partition key and sort key**
	* **Can be created at any time**

## Scan vs Query API Calls

* Query - find items in table based on primary key attribute and a distinct value to search for
	* Refine by using an optional sort key name
	* By default, query returns all attributes but the `ProjectionExpression` parameter can be used to specify attributes to be returned
	* Always sorted by sort key, default ascending. `ScanIndexForward = false` to reverse order
* Scan - Examines every item in the table
	* By default returns all attributes, use the `ProjectionExpression` parameter to specify attributes to return
* Query is more efficient than a scan
	* Scans will take longer as the table grows

### Scan performance

* Scans are sequential by default
* Can make them parallel by logically dividing a table or index into segments and scanning each segment in parallel
* Best to avoid parallel scans if table or index is incurring heavy read/write activity from other applications
* Isolate scan operations to specific tables and segregate from mission-critical traffic

### General Performance

* Set a smaller page size
	* Running a larger number of smaller operations will allow other requests to succeed without throttling
* **Avoid Scans!**
	* Design tables in a way that you can use the `Query`, `Get`, or `BatchGetItem` APIs

### Exam Tips

* Scan examines all items in a table
* Use the `ProjectionExpression` parameter to refine results
* Query
	* Finds item based on primary key attribute
	* Results sorted by Sort Key ascending, or `ScanIndexForward = false` for descending
* Try parallel scans rather than the default sequential scan
* Query operation is generally more efficient than a scan
* Design tables in a way that you can use the `Query`, `Get`, or `BatchGetItem` APIs

## Using DynamoDB API Calls

* Programatically interact with DynamoDB
* Common CLI commands
	* create-table
	* put-item (put or replaces)
	* get-item
	* update-item
	* update-table
	* list-tables
	* describe-table
	* scan
	* query
	* delete-item
	* delete-table
* Should understand these calls at a high level, differences between them and IAM permissions needed

### Exam Tips

* When to use each command listed and what they're used for
* CLI commands are making calls to a DynamoDB API
* Correct IAM permissions required to make an API call

## DynamoDB Provisioned Throughput

* Measured in Capacity Units
* Specify requirements when creating table in terms of read and write capacity units
* Write Capacity Unit - 1x write capacity unit = 1 x 1KB write per second
* Read Capacity Unit - 1x read capacity unit = 1 x strongly consistent read of 4KB per second
	**or** 2 x eventually consistent reads of 4KB per second (default)

### Exam Tips

* Provisioned throughput is measured in capacity units
* See above for read/write units

## DynamoDB On-Demand Capacity

* Pricing model - charges apply for reading, writing, storing data
* DynamoDB will instantly scale up and down based on the activity of your application
* Great for: 
	* unpredictable workloads
	* new apps where you don't know the usage patterns
	* when you want to pay for only what you use (pay per request)
* Which model should I use?
	* On-Demand Capacity
		* Unknown workloads
		* Unpredictable application traffic
		* Spiky, short-lived peaks
		* A pay-per-use model is desired
		* It might be more difficult to predict the cost
	* Provisioned Capacity
		* Read/write capacity requirements can be forecasted
		* Predictable application traffic
		* App traffic is consistent or increases gradually
		* You have more control over the cost

### Exam Tips

* Understand the difference, know when to use each pricing model

## DynamoDB Accelerator (DAX)

* A fully-managed, clustered in-memory cache for DynamoDB
* Read-heavy workloads (auctions, gaming, retail during holidays)
* Write-through caching service - data is written to DAX and DynamoDB at the same time
* Point app API calls to DAX cluster. If cache miss, DAX performs eventually consistent read on DynamoDB
* Reduces read load on DynamoDB tables
	* May be able to reduce provisioned read capacity and reduce bill
* When isn't it suitable?
	* Only uses eventually consistent reads, not strongly consistent
	* Mainly write-intensive applications
	* Apps that don't perform many read apps
	* Don't require microsecond response times

### Exam Tips

* In-Memory cache
* Improves response for eventually consistent reads only
* Data written to cache and DB at the same time
* Point API calls at DAX cluster, not table
* Cache miss results in eventually consistent read from DB
* Not suitable for write-intensive apps or ones that require strongly consistent reads

## DynamoDB TTL

* Defines an expiry time for your data
* Expired items marked for deletion within next 48 hours
* Great for removing old or irrelevant data (eg. session data, event logs, temporary data)
* Reduces cost by automatically removing old data
* 