Note: ($) = Exam Tip

## SQS

($) Only need to know at a very high level for the exam.

* Simple Queue Service
	* ($) A distributed message queue service
* Enable web service apps to quickly and reliably queue messages that one component in the app generates for another component to consume
* ($) Pull-based system (queue must be polled)
* Visibility Timeout - amount of time that an application server gets to process the message
	* If it expires, the item in the queue will be visible again for processing
* Acts as a buffer between components of application
	* Resolves scheduling issues
* ($) Guarantees messages will be processed at least once
* ($) Up to 14 days retention (configurable from 1 minute to 14 days). Default is 4 days

### SQS Features

* ($) Decouple Application Components
	* Components can run independently, easing message management between components
* Store Messages
	* Any component can store messages in the queue
	* ($) Up to 256 KB of text (any format)
* Retrieve Messages
	* Any component can later retrieve the messages programatically via the Amazon SQS API

### Understanding SQS Queue Types

* Two types of queues
	* Standard - default, provide best-effort ordering
	* FIFO - first in, first out
* Standard Queues
	* ($) Default queue type
	* Unlimited transactions per second
	* Guarantees that a messages is delivered at least once
	* ($) Best-Effort Ordering
		* Ensures that messages are generally delivered in the same order as they are sent
		* Occasionally more than once copy of a message might be delivered out of order (because of high throughput)
* FIFO Queues
	* ($) The order in which messages are sent and received are strictly preserved
	* ($) Exactly-once processing
		* Duplicates are not introduced
	* 300 TPS limit (transactions per second), but have all the capabilities of a standard queue
	* ($) Good for banking transactions which need to happen in a strict order
