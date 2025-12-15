- Google Cloud Pub/Sub is a managed, globally available messaging service that scales automatically with demand.
- Google Cloud Pub/Sub is a messaging service for exchanging event data among applications and services. A producer of data publishes messages to a Cloud Pub/Sub topic. A consumer creates a subscription to that topic. Subscribers either pull messages from a subscription or are configured as webhooks for push subscriptions. Every subscriber must acknowledge each message within a configurable window of time.

[Dataflow](https://cloud.google.com/dataflow) is a fully-managed service for transforming and enriching data in stream (real-time) and batch modes with equal reliability and expressiveness. It provides a simplified pipeline development environment using the Apache Beam SDK, which has a rich set of windowing and session analysis primitives as well as an ecosystem of source and sink connectors.

Pub/Sub is a scalable, durable event ingestion and delivery system. Dataflow compliments Pub/Sub's scalable, at-least-once delivery model with message deduplication and exactly-once, in-order processing if you use windows and buffering.

## Create a Pub/Sub topic in the current project 
```shell
gcloud pubsub topics create $TOPIC_ID
```

---
## Skills learned
- manage google cloud console products

## Useful commands used
### create a Cloud Storage bucket in the current project
```shell
gsutil mb gs://$BUCKET_NAME
```
**Note:** Cloud Storage bucket names must be globally unique.
### create an App Engine app in the current project
```shell
gcloud app create --region=$AE_REGION
```
WARNING: Creating an App Engine application for a project is irreversible and the region
cannot be changed. More information about regions is at
<https://cloud.google.com/appengine/docs/locations>.

# Push Subscription Authentication
- https://cloud.google.com/pubsub/docs/push?_ga=2.28469628.-1581002103.1690531976#authentication
- https://cloud.google.com/pubsub/docs/authenticate-push-subscriptions





---
# Useful links for Google Pub/Sub
- [Using Push subscriptions - Pub/Sub Docs](https://cloud.google.com/pubsub/docs/push)
- [Authenticate Push subscriptions in Python - Pub/Sub Docs](https://cloud.google.com/pubsub/docs/authenticate-push-subscriptions#python)
