# What publisher settings are available, and what do they do?
## Batch publishing
Batch publishing helps decrease overhead by publishing multiple messages as part of a single Publish RPC. When you call Publish using the official client libraries, your message is not published right away, but instead added to a batcher to be published alongside other messages. You can configure this behavior using the following:

- The maximum number of **messages per batch**
- The maximum **size of a batch in bytes**
- How long to hold on to a batch of messages before sending if a batch is not yet filled (**delay**).

Increasing the number of messages per batch or size of a batch decreases the number of publish requests you need to make, which translates to higher **throughput** per publish request. However, you may face increased **latency** (time between when publish is called in the client and when that publish is acknowledged by the server) if you are not filling your batches fast enough. Conversely, while decreasing batch delay can help with latency, your publish requests might end up queueing if they can’t be pushed out fast enough (lower throughput).
