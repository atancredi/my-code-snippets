# Intro

if you want to run code with a serverless developer experience on Google Cloud, you can choose either Cloud Functions, Cloud Run, or App Engine. 
App Engine has two common variants, App Engine Standard, or App Engine Flexible. 
Of these options, App Engine Flexible is the one that's different from the rest. App Engine Flexible runs on top of Compute Engine. 
The other three, App Engine Standard, Cloud Functions, and Cloud Run, are all built on the same core. 
There are implemented on top of the gVisor container sandbox. 

Cloud Run is a scalable on-demand web application platform

Cloud Run is a fully managed compute environment for deploying and 
scaling server list HTTP containers without worrying about provisioning machines, configuring clusters, or auto scaling.

Running your code on Cloud Run is not the same as on a traditional always on server. Cloud Run is request driven, uses disposable containers, and supports fast auto scaling.


First, you write your application using your favorite programming language. 
This application should start a server that listens for web requests. 
Second, you build and package your application into a container image. 
Finally, you deploy the container image to Cloud Run. 
Once you've deployed your container image, you'll get a unique https URL back.

![[Screenshot 2023-09-05 at 10.20.44.png]]

Cloud Run, then start your container on demand to handle requests and ensures that all incoming requests are handled by dynamically adding and removing containers.

For some use cases, a container-based workflow is preferred because of the more transparency and flexibility.

#### Source-based or container-based approach
If the source-based approach is followed, the source code is deployed instead of a container image: Cloud Run then builds source and packages the app into a container image.

Cloud Run can allocate up to 4 VCPUs and 8 gigabyte of memory.

# Typical use cases
A common way to use Cloud Run is to deploy one service connected to  a database like you see in the example. 
You can use this to run a REST API, a website or a web application. 
You don't even have to connect with a database if you're serving machine learning predictions, for example. 
There is no limit on container image size while memory size is limited to 8GB.

The first and most obvious applications that handle web requests like web applications, public APIs or websites. 
You can use additional services on Google Cloud like CDN and Cloud Armor to make your application performance and protect against malicious requests. 
Another sweet spot is internal line of business items. 
Cloud Run integrates with identity aware proxy, IAP so you can use your existing directory of users and don't need to worry about authentication. 
Cloud Run is not just for simple applications. 
You can run micro service based applications that communicate using direct RPC or asynchronous messages over cloud Pub/Sub.
#### event processing
![[Screenshot 2023-09-05 at 11.06.50.png]]
Cloud Run integrates with a multitude of Google cloud services. 
You can run events from Cloud Storage, BigQuery, Cloud Build, Pub/Sub and many more.

Business workflows triggered by events can be used with Cloud Run to take care of special processing needs like processing files or ordering inventory. 
You can also handle events from other Google Cloud Services and schedule short background tasks. 
Just to be sure to keep those background tasks less than 30 minutes in duration.


### Bad use cases
Cloud Run is not very well suited for long jobs that last over an hour to complete. 
If you want to handle this on Cloud Run, try to split the job up into many smaller tasks that can be handled in parallel. 
If that's not possible, start a Compute Engine VM or schedule a job on a Kubernetes cluster. 

For massive stream and batch data processing with complex workflows, you're probably better off using data flow on Google Cloud. 
It's also serverless and compatible with Apache Beam.

Finally, any workload that requires a GPU or another type of accelerator, it's not a great idea to train machine learning models on Cloud Run


# Availability
three features on Cloud Run that help you design highly available applications. 
Number 1, auto scaling the number of containers to handle all incoming requests. 
Number 2, incremental application updates, switching traffic gradually with easy rollback. 
Number 3, load balancing across zones and regions.

### all incoming requests are handled with automatic scaling

auto scaling: Cloud Run automatically increases capacity when necessary to make sure it handles all incoming requests.
Every service has an internal load balancer. 
The load balancer distributes requests over the group of available containers. 
If all containers are busy, Cloud Run adds additional containers. 
As soon as the demand decreases once more, Cloud Run stop sending traffic to these containers and shuts them down.

### Handling application updates
A common cause of service disruption are often application updates. 
These events of course, affect the availability of your application. 
Cloud Run supports immutable deployment out of the box. 
If you make a change to your application, Cloud Run creates a new revision. 
A revision is an immutable copy of your container and the service configuration. 
You can shift the traffic to the new revision gradually to reduce the impact of a failure and maintain application availability during the update. 
It is not required to take an incremental approach to deployment. 
You can also choose to send a 100 percent of your traffic to the new revision immediately.

### Regional availability
Cloud Run is a regional service. 
This is important because it affects the reliability of Cloud Run. 
A region is a Google Cloud data-center. 
For example, US center 1 is Iowa, North America. 
Every region has three or more zones. 
Zones are designed to be a single point of failure inside a region. 
This means it is unlikely that a single zone will go down and it is incredibly unlikely that all three zones go down at the same time. 
Cloud Run distribute your containers across multiple zones in one region to make sure it is resilient against the failure of a zone.

### Global Load Balancer
allows to expose a single global IP address in front of multiple regional Cloud Run services. The load balancer makes sure the traffic from a client always goes to the region closest to them.
It improves availability and decreases latency for clients worldwide.


# Concerns about serverless computing
- auto-scaling costs: it is very important to think about configuring scaling limits.
- scaling mismatch: if the CloudRun service scales to 1000 containers in under 30 seconds the downstream system might not be ready, and something that handles this must be taken into account.
- vendor reliance


# Container Images
Container images is a package with the app in it, written in any language, along with everything the app need to run.
Running a container image means that one of the programs inside the container images is being executed. The container image is an archive with files, and the container represents the running processes of the application (it only exists at runtime - if they're no running processes there is no container).
Processes in the container have access to a virtual network interface with a local IP: the application can bind to this interface and start listening on a port for incoming traffic. A process running in a container has access to its own private virtual network stack, so the application can always open a port and listen for incoming connections.

Cloud Run expects your container to listen on port number 8080 to handle web requests.
port number 8080 is a configurable default. 
If port 8080 is unavailable to your application, 
you can change the application's configuration 
to use a different port.

Cloud Run, expect your application to handle web requests. 
You don't need to provide an HTTPS server because Cloud Run handles that for you.

# How to run an application in Cloud Run
When you deploy a container image for the first time, Cloud Run creates a service, that holds the container image. There is only one container image per service, and each service has a unique HTTPS endpoints on a sub-domain for the run.app domain.
Cloud Run forwards incoming requests to a container, starting it. then it dynamically adds and removes containers as needed to handle all incoming requests (this is called auto scaling)

Services are created to enable the deployment of a containerized image. 
Every service has a unique name and then associated HTTPS endpoint. 
A service keeps track of the configuration of 
your container image and any changes you make to it. 
You'll interact primarily with a service resource to perform your tasks, such as deploying a new container image, rolling back to a previously deployed version, and changing configuration settings like environment variables and scaling boundaries.

### how does the container image get to cloud run
when an image is deployed is sent to Artifact Registry (ex Google Container Registry - GCR) as the intermediate storage location. Cloud Run can only pull container images from Artifact Registry, which is a universal package manager used to privately host container images, node.js packages or java packages.
- when ready to deploy the container image begin by pushing it to a docker repository on Artifact Registry. an unique name is given to the container image and it's ready to be deployed to a service in Cloud Run.

You open the web console or GCloud terminal interface and enter a command to create or update an existing service resource with the reference to the container image. 
Finally, Cloud Run pulls the container image from the Docker repository. 

To ensure that containers on Cloud Run start reliably and quickly, Cloud Run copies and stores the container image locally. This also reduces the startup time of large image, which is similar to smaller ones.

# Terraform
an alternative way to manage Cloud Run resources
third party tool that uses a declarative approach to provisioning and maintaining the cloud infrastructure.
You describe all of the Cloud resources needed and the links between them using a configuration language. 
Terraform builds your entire project from this specification. This is also known as infrastructure as code. 
It helps you build consistent and reproducible environments, making collaboration with a team on a shared project easier.

# Autoscaling and on-demand containers
Multiple requests can be handled by a container at the same time concurrently and Cloud Run will not shut down a container that it is handling requests unless something bad happens like an application crash. 
Cloud Run automatically increases capacity when necessary to make sure it handles all incoming requests. 
This feature is known as autoscaling and this is how it works.

Every service has an internal load balancer, the load balancer distributes requests over the group of available containers. 
If all containers are busy, Cloud Run adds additional containers to the service. 
\As soon as a demand decreases again, Cloud Runs stop sending traffic to some containers and shuts them down. 
The number of container instances in a Cloud Run service is limited to 1000 instances by default.

When no requests come to the service for awhile, Cloud Run shuts down all containers. 
A fresh container will start on-demand as soon as a new request comes in. 
You might also know this as scale to zero. 
This process is attractive for economic reasons because you're not paying for idling containers. 

The pricing mode along Cloud Run is unique because you pay only for the system resources you use while a container is handling a request with a granularity of a 100 milliseconds.

We should emphasize that you are charge for container time and not request time. 
If your container can handle 50 requests at the same time, you're paying one time and not 50 times for your container.

It's important to make sure that every background thread started while handling a request finishes processing before returning the request to the user. This is because as soon as a container is handling no requests the CPU is throttled to nearly zero, this means that your application will run at a really slow pace. Because Cloud Run shuts down and throttles idle containers, you should complete all work before you return the HTTP request. This means that any tasks you run in the background after returning an HTTP request might not finish.
The solution is to turn your background tasks into HTTP requests. 
Two products on Google Cloud help with this; ==Cloud Tasks== and ==Cloud Scheduler==.

#### Important
the entire file system of the container is in memory and disappears when the container is shut down.
This means you'll need to store persistent data in a downstream system.
For example, you can store binary data in Cloud Storage, session data in Redis, that's memory store on Google Cloud and your application data in a relational database.

## Ways to serve requests
There are a lot of options to serve requests your way. Most applications handle plain HTTP / 1 requests, like many apps do. If you need WebSockets for interactive applications, that works as well. If you want to use HTTP / 2 the more modern of performance version of HTTP, Cloud Run forwards the connection from clients all the way through application and you can use streaming responses. With the streaming protocols, there are no limits on request and response size. If your client support HTTP / 2 and your application doesn't, Cloud Run builds an HTTP / 2 connection with the client and downgrades the protocol to communicate with your application. This delivers some of the performance benefits of HTTP / 2, while you don't have to change anything.



# Container images - in depth
#### Deploy
there are two ways to deploy your application to Cloud Run. 
You can deploy your source code or you can deploy a pre-built container image. 
If you deploy source code, Cloud Run builds and packages your application using Buildpacks. 
Buildpacks is an open source tool that turns source code into container images, you can also use it on your local machine. 
If you think that's not transparent enough, you can use a lower-level tool that offers more control like Docker. 
Docker lets you build container images with a script, the script puts you in full control of the contents of the container image.

Here's what's important to remember about container images.
A container image is a package. 
It has your application and everything your application needs to run, which can include an entire runtime, assets and container configuration that points out what program to start and how. 
A minimal container Image only has one program file and a command to run it. 
Different languages may need a runtime configured as well. 
For example, Java, Python and Node.js. 
Your application might need additional system dependencies to work. 
For example, a headless browser.
Those are also included in the image. 
The container image is a self contained package with your application and everything it needs to run.

# Lifecycle of a container
- ### starting
	starting state is when Cloud Run materializes the container image and start your application.
	
	Starting begins when Cloud Run pulls a container image and ends when the container starts to serve web requests.
	Starting a container requires four steps. 
	Cloud Run corrects the container's root file system by materializing the container image. Once the container file system is ready, Cloud Run runs the entry point program of the container i.e., your application. While your application is starting, Cloud Run continuously probes a port (default to 8080) to check whether your application is ready.

- ### serving requests
	state in which the container is handling web requests
	
	Once your application starts accepting TCP connections, Cloud Run forwards incoming web request to your container. Cloud Run considers a container ready as soon as it accepts TCP connections.
	Cloud Run pulls the container image from internal storage. When a new container image is deployed Cloud Run pulls and copies it from Artifact Registry to the internal storage and every time it starts a new container it pulls the container image from there. This ensures that large container images load just as fast as tiny ones (the internal storage is largely optimized) while also insulating the service from failures in Artifact Registry.

- ### idle
	idle means the container is not handling web requests

- ### shutting down
	cloud run allows applications to be stopped gracefully by handling the shutdown hook
	Cloud Run can shut down idle containers since they are not handling requests. You can't control when a shutdown happens but you can register a lifecycle hook to make sure you can handle a shutdown gracefully.
	
	When shutting down Cloud Run sends a SIGTERM signal to the application. If the application handles the received signal, Cloud Run gives the container 10 seconds to stop gracefully (during those 10 seconds the container has full access to the CPU).
	If the application does not handle the signal Cloud Run immediately stops the container forcefully. The process just stops and disappears
	
	Finally, Cloud Run never stops a container that is handling requests, but application crashes and memory limits can bring even a container down that is serving requests and that will also include all in flight requests brought down as well.

- ### stopped
	final state of the lifecycle, where the container is stopped


# Request queueing - Cold start
If a service does not get requests for a while it scales down to zero containers. The first few requests that come in after the service scales to zero will queue while the first container starts. This is known as cold start.
The latency penalty is proportional to the start delay of your container.

Requests do not queue if there is a container available to handle the request

There is a setting that helps avoid cold starts. 
With minimum instances, you can tell Cloud Run to never remove the last container or even more than one container. 
You can tell it to keep a container idle with the minimum instances setting. 
You'll be charged for keeping a container idle, but at a reduced rate. 
If incoming traffic is more than the existing containers can handle, a new container or containers will be started to help handle the load.

# Deployment
Process used to update an application on Cloud Run.
Change your application source code, build and package your application into a container image, push the container image to Artifact Registry, deploy to the service.

Changing any of the settings of the service configuration makes Cloud Run deploy a new revision of your application. When a new revision is created Cloud Run immediately sends traffic to it as soon as it is healthy.
A revision is an immutable copy of the service resource.

If Cloud Run sees a new revision resource, it will create a new revision at run time, which is a group of containers you can send requests to. 
Cloud Run waits for the first container in that revision to finish starting. 
When Cloud Run is waiting, the old or current revision still says production traffic. 
Now, as soon as the first container in the new revision is ready, Cloud Run thinks it is ready to accept a TCP connection. 
Cloud Run sends all production traffic to it. 
The container in the previously active revision will become idle. 
That is, it won't serve requests. 
If something goes wrong, the current revision continues to serve production traffic as if nothing happened.
The old revision will eventually remove the idle containers and scale to zero. 
If your application handles a lot of traffic and takes some time to start, this scaling behaviour might not work well, and resulting request queuing. 
There is a solution. You can gradually migrate traffic
Solutions like pre-warming a revision are not available.

After you edit the service resource, Cloud Run copies the service configuration to a new resource, creates a new revision at runtime, waits for the first container to become ready, shifts all traffic to the new revision, auto-scale both the old and the new revision. By default, Cloud Run sends all traffic to the latest healthy revision.

You can also pin traffic to a specific revision rather than the latest revision, decoupling deployment of a new revision from the migration of traffic. 
This also means that if you add a new revision, Cloud Run will not automatically send traffic to that new revision.
Pinning to a revision is useful if you want to roll back to a previous revision, or if you first want to test your new revision before moving all traffic.

Cloud Run also lets you tag a revision. A tagged revision has its own unique URL. 
It's the URL of the Cloud Run service with the name of the tag added as a prefix.
A tag can also be used to point to the latest healthy revision. 
This is how you can support a preview linked to the latest revision.
You run a few confidence checks before migrating production traffic over to the new revision. 
Tags can also send traffic to one revision at a time.
Tags allow you to test or preview a diploid revision before assigning it to any traffic. 
This allows thorough testing before a new revision is used for production. 
To accomplish this type of testing, you run integration tests on your container during development. 
Deploy your container to a Google Cloud Project that you use only for staging, meaning it serves no traffic and test against a tapped revision.
Deploy the container to production once again without allowing it to serve traffic and test against a tagged revision in production. Finally, migrate traffic to the tagged revision once your tests are complete. 

Now, you might not want to deploy all traffic immediately. 
At Google, all of our deployments are gradual, and they might take days to get to 100 percent. This allows us to carefully monitor for increased error rates and find issues with scaling.

