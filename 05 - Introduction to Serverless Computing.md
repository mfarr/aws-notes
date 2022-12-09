
## Serverless 101

* Run application code in the cloud without worrying about managing servers
* Low cost - event driven, only charged when code is executed
* AWS takes care of
	* Capacity provisioning
	* Patching
	* Auto Scaling
	* High Availability
* Lambda
	* Run code as functions without provisioning servers
* SQS (Simple Queue Service)
	* Message queueing service
* SNS (Simple Notification Service)
	* Messaging service for sending text messages, emails, mobile notifications
* API Gateway
	* Create, publish and secure APIs at scale
* DynamoDB
	* Fully managed NoSQL database
* S3
	* Object storage and web hosting

## Introducing Lambda

* Supported languages - Java, Go, PowerShell, Node.js C#, Python, Ruby
* Charged based on number of requests, duration and the amount of memory used
* Extremely cost effective
* Event-driven architecture
	* Triggered by other AWS services or called directly from any web or mobile app
	* Events or User Requests
* Triggers
	* DynamoDB
	* Kinesis
	* SQS
	* Application Load Balancer
	* API Gateway
	* Alexa
	* CloudFront
	* S3
	* SNS
	* SES
	* CloudFormation
	* CloudWatch
	* CodeCommit
	* CodePipeline

## API Gateway

* Publish, maintain, monitor, secure APIs at any scale
* Single endpoint for all client traffic interacting with backend of application
* Supports multiple versions of your API
* Supported types
	* RESTful
	* WebSocket
* RESTful
	* Optimized for serverless and web applications
	* Stateless
	* Supports JSON

## Version Control with Lambda

* $LATEST is always the latest version of the code uploaded to Lambda
* Can create multiple versions of a function and use aliases to reference
	* Alias points to a specific version of the function
	* Alias referenced by ARN
		* eg. `arn:aws:lambda:us-east-1:123345456:function:mylambda:Prod` *or* `arn:aws:lambda:us-east-1:123345456:function:mylambda:$LATEST`
* Weighted alias
	* Send traffic to different versions of a Lambda based on weights (%)
		* Ie. send 95% of traffic to Prod alias, 5% to Test alias

## Lambda Concurrent Executions Limit

* (Exam) Don't need to memorize lots of limits, just be aware that there is a concurrent execution limit for Lambda
* Limit for all function executions in a region per account (default is 1000)
	* If running a large serverless site, you will likely hit this limit
	* Contact support to increase
* **Reserved Concurrency** guarantees that a set number of executions which will always be available for your critical function, however this also acts as a limit

## Lambda and VPCs

* Some use cases require Lambda to access resources which are inside a private VPC
	* eg. read or write to an RDS database
* By default, Lambda will not be able to access resources in a private VPC
* To enable access to a private VPC you need:
	* Private subnet ID
	* Security group ID with required access
	* Lambda uses this to set up ENIs (Elastic Network Interface) using an available IP address from private subnet
* Add VPC information to Lambda config using the `vpc-config` parameter or when creating a function (via console, etc.)

## Step Functions

* Provide a visual interface for serverless applications allowing you to build and run serverless applications as a series of steps
* Each step in app executes in order as defined by your business logic
	* Output from one step often becomes input for next step
* Manage the logic of your application
	* Including sequencing, error handling and retry logic
	* Will also log the state and outcome of each step
* Sequential Workflow
	* Steps happen one after another
* Parallel Workflow
* Branching Workflow

## Comparing Step Function Workflows

* Standard Workflows
	* Long-running, durable and audit-able workflows that may run for up to a year
	* At-Most-Once Model
		* Tasks are never executed more than once unless explicitly specify retry actions
	* Non-Idempotent Actions
* Express Workflows
	* Short-Lived
	* Up to 5 minutes, great for high-volume, event-procesing-type workloads
	* At-Least-Once Model
		* Tasks may be run more than once or if you require multiple concurrent executions
	* Idempotent Actions
	* Synchronous vs. Asynchronous
		* Synchronous - waits until it completes and then returns a result
		* Asynchronous - confirms workflow has started, result can be found in CloudWatch Logs

## Understanding X-Ray

* Analyse and debug distributed applications, provides a visualisation of application's underlying components
* Integrates with AWS Services, applications and API calls made to AWS services using the AWS SDK
* X-Ray SDK, X-Ray Agent/Daemon on EC2 instance running application
* X-Ray Service Map provides an end-to-end view of requests as they travel through an application
	* Latency
	* HTTP Status Codes
	* Errors

## X-Ray Configuration

* Annotations & Indexing
	* Annotations - key-value pairs that are indexed for use with filter expressions, indexing and searching based on pairs
* For ECS, run the X-Ray daemon in its own Docker image, running alongside your application

## Advanced API Gateway

* Importing APIs into API Gateway
	* OpenAPI (formerly Swagger) format
	* Create or update an existing API
* Legacy Protocols
	* SOAP
		* Can use API Gateway to transform the XML response to JSON or use API Gateway as a passthrough to SOAP API

## API Gateway Caching & Throttling

* Caching
	* Caches endpoint response for a specified TTL in seconds (default: 300)
* Throttling
	* Prevent API from being overwhelmed by too many requests
	* Default limit is 10,000 requests/s/region
	* Concurrent requests - 5000 requests across all APIs per region
	* 429 Too Many Requests error when exceeded