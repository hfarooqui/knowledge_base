# Application Services

## Simple Queue Service
 - Web service that gives you access to message queues that can be used to store messages while waiting for an instance to process them
 - Distributed Queue system that enables web service applications to quickly and reliably queue messages that enable one component in the application to generate to be consumed by another component
 - Queue is temporary repository of messages that are awaiting processing. It acts as buffer between components producing and consuming data.
 - **PULL** based system
 - Using SQS you can decouple the applications so that they can run independently easing message management between components
 - Any component of distributed application can store messages in queue. Message can contain 256KB on text in any format. Components can later retrive messages using SQS API's
 - You can create Auto Scaling Group to monitor SQS. If the message count goes above or below the threshold you can increase or decrease the number of instances to bring elasticity to application
 - Messages can be kept in the queue from 1 minute to 14 days
 - Default retention period is 4 days
 - SQS gurantees that your message will be processed atleast once
 
 ### Visibility timeout
 Amount of time message is invisible in SQS queue after reader picks up that message. 
 - Provided that job is processed before the visibility timeout expires the message will than be deleted from the queue. If the message is not processed within that time, it will become visible to be picked up by another reader. 
 - Default visibility timeout is 30 sec
 - Maximum visibility timeout is 12 hours
 
 ### Standard Queues 
  - Default queue type
  - Allows you to have unlimited number of transactions per second
  - Messages are delivered atleast once. Ocassionally more than one copy of message might be delivered out of order
  - Messages are generally delivered in the same order as they arrive
 
 ### FIFO queues
 - Messages are STRICTLY delivered in the same order as they arrive
 - Messages are delivered ONCE and remains in the queue until consumer processes and deletes it
 - Duplicates are not introduced in the queue
 - Limited to 300 transactions per second
 - 
