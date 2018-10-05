# Application Services

## Simple Queue Service (SQS)
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

![SQS1](https://s3.amazonaws.com/hfcontents/kbimages/SQS1.png "SQS1")

![SQS2](https://s3.amazonaws.com/hfcontents/kbimages/SQS2.png "SQS2")

 ### Standard Queues
  - Default queue type
  - Allows you to have unlimited number of transactions per second
  - Messages are delivered atleast once. Ocassionally more than one copy of message might be delivered out of order
  - Messages are generally delivered in the same order as they arrive
 ![SQS_Standard](https://s3.amazonaws.com/hfcontents/kbimages/SQS_Standard.png "SQS_Standard")

 ### FIFO queues
 - Messages are STRICTLY delivered in the same order as they arrive
 - Messages are delivered ONCE and remains in the queue until consumer processes and deletes it
 - Duplicates are not introduced in the queue
 - Limited to 300 transactions per second
![SQS_FIFO](https://s3.amazonaws.com/hfcontents/kbimages/SQS_FIFO.png "SQS_FIFO")

  ### Visibility timeout
 - Amount of time message is invisible in SQS queue after reader picks up that message. 
 - Provided that job is processed before the visibility timeout expires the message will than be deleted from the queue. If the message is not processed within that time, it will become visible to be picked up by another reader. 
 - Default visibility timeout is 30 sec
 - Maximum visibility timeout is 12 hours

### SQS Long Polling
- Way to retrive messages from SQS queue
- Regular short polling returns immediately even if message queue being polled is empty
- Long polling doesn't return a response until a message arrives in message queue or long poll times out
- Long polling is economical

## Simple Workflow Service (SWF)
- Makes it easy to coordinate work across distributed application components
- Tasks represents verious processing steps in an application which can be performed  by executable code, web service, human interaction and scripts
- Maximum workflow can be one year and it measured in seconds
![SWF1](https://s3.amazonaws.com/hfcontents/kbimages/SWF.png "SWF1")

### SWF Workers
- Programs that interact with SWF to get tasks, process received tasks, return the results

### SWF Decider
- Program that controls coordination of tasks i.e; their ordering, concurrency and scheduling according to application logic

- SWF Brokers interaction between Workers and Decider
- It allows decider to get consistant views into the progress of the tasks and initiate new tasks in an ongoing manner
- At the sametime SWF stores tasks, assigns them to workers when they are ready and monitors their progress.
- It ensures tasks are assigned only once and are not duplicated
- Workers and decider does not have to keep track of the execution state. They can run independently and scale quickly

### SWF Domains
 - Your workflow, its execution and activity types are all scoped to a domain
 - Domain isolate set of types, executions and task lists from others within the same account
![SWF_Domain](https://s3.amazonaws.com/hfcontents/kbimages/SWF_Domain "SWF_Domain")

## SWS vs SQS
 - SWF represents task oriented API's whereas SQS represents message oriented API's
 - SWF ensures that task is assigned only once and never duplicated. Whereas with SQS you need to handle duplicate messages and may also need to ensure that message is processed only once
 - SWF keeps track of all the tasks and events in application. Whereas with SQS you need to implement your own application level tracking especially if your application uses multiple queues

## Simple Notification Service (SNS)
- Web service that makes it easy to setup, operate and send notifications from cloud
- Instantenous PUSH based delivery (no polling)
- Flexible delivery over multiple protocols
- Provides developers with highly scalable, flexible and cost-effective solution to **PUBLISH (PUSH)** messages from an application and immediately deliver them to Subscribers or other applications
- Push notifications to Apple, Google, Amazon and Windows devices
- Besides PUSHING notifications to mobile devices SNS can also deliver notifications by SMS text or email to SQS or to any HTTP endpoint
- SNS endpoint can also trigger lambda functions. When the message is published to SNS topic that has lambda function subscribed to it, the Lambda function is invoked with the message payload as input parameter. Lambda funciton can then manipulate the information in the message and send notification to other SNS topic or AWS services
- SNS allows you to group multiple recipiants using topics.
- **TOPIC**: Access point for allowing recipients to dynamically subscribe for identical copies of same notification. One topic can support deliveries to multiple endpoints. For example you can group togather iOS, ANdroid and SMS recipients. When you publish once to the topic SNS delivers appropriately formated copies of your message to each subscriber
- To avoid message loss all the messages published to SNS are stored redundantly across multiple AZ

## SNS vs SQS
- Both are messaging services in AWS
- SNS: Push based
- SQS: Pull based

## Elastic Transcoder
- Allows you to convert media files from original source format into different format that will play on smartphones, tablets, PC's
- Pay based on the minutes that you transcode and the resolution at which you transcode
![Elastic_Transcoder](https://s3.amazonaws.com/hfcontents/kbimages/Elastic_Transcoder.png "Elastic_Transcoder")
