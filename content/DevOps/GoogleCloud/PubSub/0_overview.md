- Pub/Sub clients publish messages to **topics**.
- Pub/Sub clients receive messages from **subscriptions**. You can create multiple subscriptions for the same topic, in which case each subscription gets a copy of all of the messages in the topic. Some beginners assume that one would pull messages from a **topic**, which isn’t possible — **topics** are for publishing only. The **subscription** is where you will receive all messages.
- Your application can create multiple **Pub/Sub clients** to publish to a topic, across different processes. (With some caveats mentioned below.)
- Your application can also create multiple **subscriber clients** for a single subscription, in which case message delivery is load balanced across all of them. Messages are delivered to one client on the **subscription**, not all of them.

# Best Practices
## Subscriptions
- Streaming pull is the optimal and recommended way of receiving messages from Pub/Sub.