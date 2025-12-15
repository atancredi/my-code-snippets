# How do I configure IAM on my topic or subscription?

## Fine grained [access control](https://cloud.google.com/pubsub/docs/access-control) can be achieved by setting roles and permissions at the topic and subscription level.

There may be times that you want to limit what a user or a third party service can do with your Pub/Sub resources.

For example, a downstream message consumer identified by a certain service account, perhaps from a different GCP project, should only have permissions to subscribe but not to update a subscription’s message retention policy. This can be achieved by setting the IAM policy on that subscription such that its policy member, the service account in this case, does not have a role with the `pubsub.subscription.update` permission. An example of a role with this permission is `roles/pubsub.editor`. An example of a role without this permission is `roles/pubsub.subscriber`.

Setting the IAM policy on a topic or subscription often involves three steps:

- Getting the existing IAM policy for a topic or a subscription in a JSON or YAML file.
- Updating the JSON or YAML file with changes you would like to make: add/remove a member, add/remove a role
- Applying the changes in the JSON or YAML file to your topic or subscription.

Here is a summary of the [predefined roles](https://cloud.google.com/pubsub/docs/access-control#roles) that you can add to a topic or subscription’s IAM policy. If you need to, you can also customize roles by mixing and matching [permissions](https://cloud.google.com/pubsub/docs/access-control#required_permissions). (Also see [GCP IAM roles explained](https://medium.com/google-cloud/gcp-iam-roles-explained-af84955346e7).)