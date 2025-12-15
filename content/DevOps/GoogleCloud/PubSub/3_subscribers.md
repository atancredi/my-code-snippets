# What subscriber settings are available, and what do they do?

### **Flow control** 
Useful for limiting how many messages your subscribers will pull from Pub/Sub. If your client pulls too many messages at once, it may not be able to process all of the messages, potentially leading to expired messages and an increasing message backlog. Messages expire after a certain time to allow messages to be redelivered to healthy subscribers if a single subscriber fails. You can limit both the number of messages as well as the maximum size of messages held by the client at one time, so as to not overburden a single client.

### **Concurrency control**
Allows you to configure how many threads or [streams](https://cloud.google.com/pubsub/docs/pull#streamingpull) are used by the client library to pull messages. Increasing the number of threads or streams allows your client to pull messages from the Pub/Sub server more rapidly. However because each stream requires additional overhead, having too many streams open can actually lead to decreased performance. [Each streaming pull connection can handle 10 MB/s](https://cloud.google.com/pubsub/quotas#resource_limits) so consider your message throughput when changing this value. Increasing the number of callback threads allows you to process more messages concurrently.

# What is a push subscription and when do I use it?
Pub/Sub delivers messages to a predefined HTTPS address (also called a push endpoint) as a HTTP POST request.

Unlike a pull subscription, where you actively ask Pub/Sub for messages, a push subscription lets you configure a push endpoint, and Pub/Sub delivers messages to this push endpoint.

You should consider a push subscription if any of the following applies:

- You are not able to include any client library code in your subscriber application.
- Your subscriber is not allowed to make any outgoing requests.
- Your subscriber shall handle messages from different subscriptions the same way, i.e. you use the same push endpoint for different subscriptions.

Message acknowledgement in push subscriptions is achieved differently from that in pull subscriptions. In pull, your application code must explicitly call the **ack**nowledge (**ack**) method on a received message. In push, a successful response code (one of 101, 200, 201, 202, 204) for a push request serves as an ack.

When a push request gets other response codes, indicating that the request has failed, a message is considered nack’ed, and the following will happen:

- Pub/Sub will try to re-deliver the message with exponential backoff. This means you should expect longer periods between subsequent deliveries.
- Pub/Sub will also lower the limit of outstanding messages, i.e. messages that Pub/Sub has sent out but have yet been ack’ed or nack’ed.

Both pull and push subscriptions meet high-throughput needs. But the maximum throughput per subscription per region for the push mode of delivery is half of that of the pull-delivery mode per subscription per region. To learn the exact quote units, check out the reference page for [quota limits](https://cloud.google.com/pubsub/quotas#quotas).



