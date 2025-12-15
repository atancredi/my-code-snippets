# Service-to-Service communication
Modern applications are often split into multiple services and in some instances, each of these services is created to handle specific tasks within the application.
A common way to use Cloud Run is to deploy one service connected to multiple back-ends and APIs.

Another way is to use multiple services that have distinct responsibilities. 
That is a microservice architecture. 
Doing so might result in several benefits; 
Improved scalability because every service can scale independently. 
Improved collaboration and development speed if distributed teams build and deploy their own services independently. 
Increased robustness if one service breaks another service can continue to work. 
There were various trade-offs involved in making this architectural design choice. Cloud Run supports both design patterns, but you are limited to the system resources that are available on a per-container basis. 
Cloud Run does not require you to break down your application into multiple services but if you do, you need a way for those services to communicate.

There are two basic patterns of service-to-service communication:
- ### Request and Response
	one service sends a request and waits for a response to be returned.
	This is a synchronous approach because the sender *waits* until the request successfully completes.
	If you want to send a request from one Cloud Run service to another, you'll need to know what URL to send the request to. If you use the default URL, this URL is unique for every combination of service, project, and region.
	The recommended and most portable solution for URL discovery is to pass service URLs as a configuration in the environmental variables.
	Be careful though, not to pass secrets like for example, API keys and passwords as environment variables. Doing so is a big security risk. Instead, to pass sensitive configuration data, use the integration between Cloud Run and Secret Manager.
	Alternatively, you can call to the Cloud Run Admin API to request a list of all services in a project, but it's not a preferable approach.

- ### Publish and Subscribe
	asynchronous approach: one service sends or publishes a message to a message broker that then copies and forwards the message to one or more *subscribers*.
	Google Pub/Sub is a message broker that asynchronously sends message between services

# Publish and Subscribe with Pub/Sub
This is the runtime model of Pub/Sub: a publisher sends a message to a topic on Pub/Sub, Pub/Sub, then copies the message and guarantees delivering to one or more subscribers.

Both the publisher and the subscriber can be a cloud run service but that's not a requirement a publisher can be any process that has access to the Pub/Sub API. 

A subscriber can be any https endpoint or a process with access to the Pub/Sub API.
another key property of Pub/Sub is that the publishers are decoupled from the subscribers the publisher is not waiting for confirmation and there's no need for a read receipt by design.

One pub sub topic can have multiple subscribers, each subscriber being configured separately using the subscription resource.
included in the subscription setting are for example:
- the https endpoint to send messages to
- the retry configuration (if messages should be delivered in order or potentially out of order, which decreases latency)
- whether to apply a filter to messages
All of these settings are unique and can be set independently for every subscription.

To allow to cloud run services to communicate through Pub/Sub this is what you'll need to do:
- create a topic that the publishing cloud run service can send message to
- create a push subscription and attach it to the topic
- configure the push subscription to forward messages to the HTTPS URL of the subscribing Cloud Run service.

#### What happens when the publisher sends a message?
1. The publisher publishes a message to a topic where the message queues
2. Pub/Sub copies the message to all subscriptions that are attached to the topic
3. The push subscription finally forwards the message to the subscribing cloud run service, putting the message in the body of the HTTP POST request

## Communication styles supported by Pub/Sub
- #### One-To-Many
	you can have a topic with multiple subscribers, where one topic is pushed to multiple URLs
- #### Many-To-One
	multiple publishers can post to a single topic and a single push subscription handles the events
	An example use case is where multiple services are publishing events and one service exists to aggregate all events in a data store
	A similar but slightly different approach is where multiple topics/subscription pairs push messages to the same HTTPS URL. This is useful if you want to send messages to the same service but two different paths or have a different retry configuration for different publishers.
- ### Many-To-Many
	all services post and read events to and from a shared channel where multiple message filtering happens on the service
- ### One-To-One
	also called direct invocation, where you can create a topic and a subscription for two services that want to communicate with one another.

One of the benefits of using Pub/Sub over sending a direct message is that the publisher can send the message and then he doesn't need to wait for the result and handle errors with re-tries. If the subscriber is temporarily unavailable Pub/Sub still guarantees that the message will be delivered at least once.


## How Pub/Sub decides a message delivery is successful
![[Screenshot 2023-09-12 at 11.01.26.png]]
To deliver a message to a subscriber Pub/Sub sends the requests. If the request times out it retries the request, also if the response is not successful.
The default deadline is 10 seconds, which can be increased up to 10 minutes.

Pub/Sub decides if a request is successful by looking at the status code returned.

## Retries and duplicate delivery
Retries can use exponential backoff. This means that Pub/Sub increases the delay before retrying every time, up to a 10 minute interval between retries.
For retention duration a subscription tries to deliver a message for 7 days and will discard the message if a continuation fails.

In order to avoid losing undeliverable messages  create a new topic - the one that should receive the undeliverable messages - and create or update a subscription and set the dead letter topic. You can also set the maximum number of delivery attempts (7 days is default).

Do note that you can't have both message ordering and catching undeliverable messages enabled at the same time.

### Duplicate delivery
the application code must be prepared to handle the same message twice, or even the same message twice at the same time.
This might happen because Pub/Sub is a distributed system. Even if a timeout occurs and Pub/Sub retries the message, your code might have already processed the message fully prior to the timeout.
Your own container might exit while it is handling a request, due for example to memory pressure. In this case Pub/Sub retries the message even if the message has been fully processed already
If you retry and handle the same message twice it might be fine, but sometime it has real-world consequences.

If you publish a message to a topic, Pub/Sub assigns it a unique message ID. Even if a subscription retries the message, the message ID remains the same

To make your code resilient against retries use a lease to handle the message exclusively. 
This is how this works. As you receive the message, persist the message ID with the state in progress, and store a timestamp of when the lease expires. If you fully handle the message, update the record with the state completed. Reject all messages that have an existing lease that did not expire.


## IAM access restriction
If you want to make sure a service can only receive a message from a specific push subscription you can use IAM to restrict access to the cloud run service (the push subscription has a service identity).
- create a service account
- configure the service account and the identity of the post subscription
- create a policy binding that binds the service account to the Cloud Run role invoker
- add the binding to the IAM policy that attaches to the cloud run service
if those steps are completed Pub/Sub automatically adds a Google signed open ID token to the request for the pub subscription to the Cloud Run Service and the IAM will reject all other requests

## Summary
The push subscription guarantees at least once delivery and re tries failed messages, to avoid discarded messages configure a dead letter topic and monitor the messages to that topic.
