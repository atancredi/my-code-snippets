from this [Coursera Course]([www.coursera.org/learn/application-development-with-cloud-run])

Routing requests through the global load balancer brings extra features and options at the expense of a higher fixed fee.

The three parts of the Global HTTPS Load Balancer are the frontend, the URL map, and the backend services.
## components of the Global Load Balancer
- ### Frontend
	This is where the application interacts with requests.
	A single static ip is provided and HTTPS traffic can be sent to it.
	The second component is the HTTPS proxy: it can be configured with a custom SSL certificate or Google can manage it.
	The third part of the frontend is the forwarding rule: if a static IP with an HTTPS proxy is created, they need to be bound together with a forwarding rule. In this way Google Cloud knows to forward incoming TCP connections to the HTTPS proxy, which then forwards requests to the URL map
	The frontend forwards the traffic to the URL map
- ### URL map
	it has path based routing configuration to send traffic to one or more backend services
	A URL map can match paths or hosts in the request to send traffic to different backend services. This is how different Cloud Run services can be combined with a bucket to host static assets, or how certain requests can be forwarded to a service running elsewhere.
	The URL map is also useful to isolate parts of the application from each other.
- ### Backend service 
	It contains all of the services that handle the applications incoming requests
	A Cloud run service can be a backend, as well as other services on Google Cloud.
	A backend service holds backends of the same type (it has little sense to put a Cloud Function and a Cloud Storage bucket together in one backend service, for example)
	The backend service is a necessary abstraction, because the backends are regional and the URL map is global. It is possible to put multiple backends from different regions in one backend service.
	Examples:
	- a Compute Engine instance group with VMs
	- a Kubernetes service in GKE
	- an external endpoint (the external HTTPS Load Balancer will proxy the requests). This is particularly useful when incrementally rebuilding an application while still hosting parts on premise.
	Note that Cloud Storage Buckets can be added to a URL map but not to a backend service.

It is called Global HTTPS Load Balancer because it is possible to deploy an application n multiple regions and send client requests to the region closest to them (cross region load balancing).

## Cloud CDN
### why use it?
CDN: content delivery network
it serves cacheable content closer to clients (closer in terms of network distance). Cacheable content is stored on an edge point of presence, while dynamic content is still served by the backend service.
A CDN improves the availability and performance of an application.

The content in the Cloud CDN store expires, and the store has a limited total capacity

Any backend service supports Cloud CDN: this means that services that use Cloud CDN can be combined with services that don't via path-based routing.

It's important to configure correctly the CDN for it to be effective.
Cloud CDN has three modes that allows to choose what content is cacheable.
- use cache headers
	if the application response to a web request, it needs to tell Cloud CDN using a header how long it's allowed to store the response before it needs to fetch it again
- catch-all static content
	in this case caching can be prevented by setting no-cache response headers
- cache all for an hour
	NB must be very careful when enabling this mode because there is no way to override the cache from the application code - so this can result in a lot of cache storage being used.

The application gains three benefits from using Cloud CDN: contents availability is increased, serving content through Cloud CDN improves website performance. Finally, if Cloud CDN serves more requests, it means that Cloud Run is not handling them, which can be cheaper for certain use cases.

## Cross regional load balancing
Latency is one of the reasons why an application must be served in multiple regions, so this represents another great reason to adopt the Global HTTPS Load Balancer with Cloud Run
In other words, if the application is serving a global audience, you must be aware of network latency.

One solution, is to deploy your applications in multiple regions and use cross region load balancing. There's also a reliability benefit from using the global https load balancer because you're insulating yourself from the possible, but very rare event of a cloud run region failure.

Most applications need to store and retrieve data in order to be useful, and it's very likely that you want users in one region to see the same data as users in other regions.
If you use regional databases, for example, using cloud SQL, it is challenging to make sure the data is consistent across regions, especially if they are far away from each other.
One solution to this problem is to use a multi regional database spanner is a distributed multi regional SQL database. 
It provides features such as global transactions and strongly consistent reads.