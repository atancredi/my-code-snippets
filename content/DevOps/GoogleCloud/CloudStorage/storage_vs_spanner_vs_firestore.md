
# Data persistency on Google Cloud Platform
## Cloud Storage
It has some advantages: unlimited storage with no min object size, high durability (99.9% annual durability), geo redundancy (if the data is store in a multi- or duo- region), multiple storage classes for different access patterns with different costs models.
Cloud Storage allows worldwide storage and retrieval of any metadata at any time. You can use Cloud Storage for a range of scenarios. Serving static website content like images and downloads, storing data for backups, and disaster recovery. Distributing large data objects to users via direct download. Ingesting large amounts of data for later analysis. For example, using BigQuery or ingestion into Dataflow.
## Cloud Spanner
Cloud Spanner is a proprietary, fully managed alternative to Cloud SQL. Cloud Spanner offers horizontal scalability with up to three nines of availability and has zero scheduled downtime. Cloud SQL has 99.95 percent availability and does not count downtime of less than a minute with maintenance for patching.
You can use it like a traditional relational database. 

As an application developer, Cloud Spanner offers you the use of client libraries for most programming languages and existing tuning can use to provide a JDBC drivers for connectivity with third-party tools. The lowest monthly price of Spanner starts around $65 per month.
## Firestore
Firestore is a serverless document database that scales to meet any demand. 
Think Cloud Run, but as a database. 
Firestore supports real-time data synchronization between clients, which enables developers to create interactive experiences in mobile and web applications. For some use cases, you don't need to develop back-end code because Firestore supports validation rules and access control rules as a feature in the database. Firestore has up to 99.9999 percent availability, which translates to less than 26 seconds of downtime per year. 