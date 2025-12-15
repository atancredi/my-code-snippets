# Google Cloud Fundamentals

# Week 1: Course Introduction

Google cloud: compute, storage, big data, machine learning, application services

course goals: identify purpose and value of GC products and services; choose among and use application deployment envs on GC; choose among and use GC storage options; interact with GC services; describe ways in which customers have used GC.

# Week 2: Introducting Google Cloud

## Cloud computing overview

traits of cloud computing: computer resources on-demand and self-service; access to said resources from anywhere over the internet; the provider of the resources pools them and allocates to the users; resources are elastic, allowing flexibility customer-side; customers pay only for what they use.

## IaaS and PaaS

IaaS: infrastructure as a service; PaaS: platform as a service

IaaS provide raw compute, storage and network capabilities, whilst PaaS provides code libraries that access the infrastructure that application needs. In the first case the customer pays for the allocated resources, in the second they pay for the resources actually used.

Serverless cloud computing allows developers to concentrate on code by eliminating the need of any infrastructure management.

SaaS or software as a service is an application that is not installed on the local computer, but rather runs on the cloud.

## The Google Cloud network

designed to get to the customer the highest possible throughput, with lowest possible latencies. It consist of 100+ content caching nodes worldwide, where high deand content is cached for quicker access, distributed in 5 major areas: North America, South America, Europe, Asia and Australia.

The location of the app affects directly its availability, durability and latency (measurement of the time a packet of inforation takes to travel from source to destination). Any location is divided into regions, and any region is divided into zones.

# Week 3: Resources and Access in the Cloud

## Resource hierarchy

4 levels: from the bottom up

1. Resources: virtual machines, cloud storage buckets, tables in big query etc etc
2. Projects: resources are organized into projects
3. Folders: projects can be organized into folders and even subfolders
4. Organization node: includes all the resources, projects and folders owned by the organization

This hierarchy relates directly to how policies are managed and applied on GC

Policies can be defined at Project, Folder or Organization node level. Some services also allow to apply policies to Resources.

Policies are inherited downwards.

### LVL 02 Projects

Projects are the basis for enabling and using google cloud services: manage APIs, billing, adding collaborators and other google services.

Each project is a separate entity that lives under the Organization node. Projects hold resources, each of which belongs to just one project. Project can have different owners and users, and are billed and managed separately.

Each projects has 3 defining attributes: Project ID (Globally Unique IDentifier GUID, assigned by GC but mutable during creation and immutable after creation; used in different context to inform GC of the exact project to work with) , Project name (user created, not unique and mutable) and Project number (unique, assigned by GC and immutable- mainly used by google to keep track of resources).

the RESOURCE MANAGER TOOL is an API designed to programmatically manage projects: it gathers a list of projects for an account, creates new projects, updates existing projects, deletes projects, recovers previously deleted projects and can be accessed through RPC API and REST API.

### LVL 03 Folders

folders let the user assign polices to resource at a level of granularity arbitrarily chosen. Resources in a folder inherits the same policies as the folder.

A folder can contain either a project or another folder (or both)

Folders can be used to group projects under an organization in a hierarchy, also by delegating administrative rights. 

To use folders an organization node must be used

### Top level: organization node

it is possible to design a specific user as “organization policy administrator” and another as “project creator”. If the company already has a google workspace domain project will automatically belong to the corresponding organization node, otherwise Cloud Identity can be used to generate one. Once created, allows all the users to create projects, folders and billing accounts.

Both folders and projects are considered to be children of the organization node.

## Identity and Access Management (IAM)

IAM helps with the task of restricting access to certain projects, folders and resources, by letting the administrators apply policies that define “who” (can be a google account, a google group, a service account ora a Cloud identity domain) can do what (defined by the “role”, intended as a collection of permissions) on which resources.

3 kinds of roles: basic (they affect all resources in a project. includes owner - can view and edit both the project and its policies/billing informations, editor - can only view and edit the project, viewer - can only view the project, and billing admin - that can view the project and edit only the billing informations), predefined (specific google cloud services provides sets of predefined roles and even define where those roles can be applied) and custom.

Many companies use the “least privilege model” where every user has the bare minimum amount of roles needed to do their jobs.

About custom roles: user will need to manage the permissions that define the custom role created, and custom roles can only be applied to either project level or organization level.

it is good to know that IAM policies implemented by lower-level policies can override the ones defined at a higher level

## Service accounts

useful for giving permissions to a Compute Engine virtual machine rather than a person.

if i have an app running in a VM that stores data in cloud storage, but i don’t want anyone on the internet to have access to that data, just the VM: i can use a service account to authenticate it on the cloud storage

they are named with an email address but instead of a pwd uses cryptographic keys to access resources.

they do need to be managed: a service account is both an identity and a resource, so it can have their own IAM policy

## Cloud identity

the basic flow for auth starts with gmail for logging into the google cloud console, and then using google groups to collaborate with teammates. this approach is easy but it can lead to problems because the member’s identities are not centrally managed (for example if someone leaves the organization with this approach there is no easy way to remove access to the cloud resources to a user immediately).

with google cloud identity organizations can define policies and manage users/groups using the GAC.

it’s possible to log in and manage google cloud resources using the same credentials used in existing LDAP systems or Active Directory; when an user leaves the organization GAC can be used to disable his account and remove them from groups right when they leave.

available in free and premium editions (premium can also manage mobile devices), and it’s already available to google workspace customers in GAC

## Interacting with google cloud

4 ways to interact:

1. cloud console
    
    simple web-based GUI for deploying, scaling and diagnosing production issues.
    
    you can find your resources, check their health, have full management control over them and set budgets to control how much you spend on them
    
    it provides a search facility to quickly find resources and connect to instances via SSH in the browser
    
2. cloud SDK and cloud shell
    
    SDK is a set of tools that can be used to manage resources and applications hosted on google cloud
    
    - gcloud tool: main CLI
    - gsutil: provides access to cloud storage from the command line
    - bq: CLI for BigQuery
    
    Cloud shell provides CLI access to cloud resources directly from a browser: it is a Debian-based virtual machine with a persistent 5GB home directory for making easy to manage GC projects and resources. obviously Cloud shell provides the full cloud SDK and other utilities, always up to date and fully authenticated.
    
3. APIs
    
    control google cloud with custom code.
    
    it also provides Google Cloud Libraries and Google API Client Libraries in the most popular languages to reduce the effort put into the task of calling Google Cloud from the code.
    
4. Cloud Console Mobile App
    
    start, stop and use SSH to connect into Compute Engine instances, and see logs
    
    stop and start Cloud SQL instances
    
    administer application deployed on App Engine, with features as viewing errors, rolling back deployments and changing traffic splitting
    
    the app also provides alerts and incident management, as well as up-to-date billing information for projects and billing alerts for projects that are going over budget.
    
    Customizable graphs can also be set up, showing info such as CPU usage, network usage, RPS (requests per second) and server errors
    

# Week 4: Virtual Machines and networks in the Cloud

## Virtual Private Cloud networking

VPC is a secure individual private cloud computing model hosted within a public cloud, like Google Cloud. Customers can run code, store data, host websites and do anything that can be done in a private cloud. But this private cloud is hosted remotely by a public cloud provider: VPC combine the scalability and convenience of public cloud computing with the data isolation typical of private cloud computing.

VPC networks connect Google Cloud resources to each other and to the internet. this includes segmenting networks, using firewall rules to restrict access to instances and creating static routes to forward traffic to specific destinations

Google VPC networks are global and can have subnets (a segmented piece of the larger network) in any GC region worldwide. subnets can even span the zones that make up a region: this architecture makes it easy to define network layouts with global scope.

Resources can even be in different zones on the same subnet.

The size of a subnet can be increased by expanding therange of IP addressess allocated to it. doing so won’t affect virtual already configured VMs. For example, imagine a VPC with one network that currently has one subnet defined in GC’s us-east1 region. if the VPC has 2 compute engine VMs attached to it, it means they are neighbors on the same subnet, even though they’re in different zones (us-east1-b and us-east1-c but IPs 10.0.0.2 and 10.0.0.3). This capability can be used to build solutions that are resilient to disruptions, yet retain a simple network layout

## Compute engine: Google’s IaaS solution

With Compute Engine users can create and run VMs on Google infrastructure, with no upfront investments and with thousands of virtual CPUs that can run on a system that’s designed to be fast and to offer consistent performances. Each VM contains the power and functionality of a full-fledged operating system, and they can be configured much like a physical server, by specifying the amount of CPU power, memory and storage needed (as well as storage type and OS)

[0.42]