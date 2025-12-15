(from https://www.coursera.org/learn/application-development-with-cloud-run)

Now that you have a clear understanding of what a container images. 
Let's explore how to build and package an application into a container image using Docker. 
In order to build and package your application into a container image, this is what conceptually needs to happen. 
You need to install system dependencies if your application depends on those. 
You need to install a run time, for example, node.js or python. 
You need to download application dependencies, NPM install, GoGet, pip install or invoking your package manager of choice. 
You need to compile the binaries or process slash bundle the source code. 
You then need to package the files into the application image and set the container configuration. 
Docker lets you express this build process using a script called Docker file. 
Docker files offer a low level approach that offers transparency and flexibility at the cost of complexity. 
The Dockerfile is a manifest that details how to turn your source code into a container image. 
Next, we'll look at two different approaches to building and packaging your application images. 
Using your choice of build packs or Docker. 

Build packs are different from Docker in the sense that they provide a hands off and convenient approach to build a container Images by using heuristics to build and package the source code. 
If it sees a package touch base on file, it knows to run NPM install. 

\Let's first dive into Docker. 
Doctor is a container engine. 
You can use it to run containers on your local machine. 
You can also use it to build containers. 
Docker bill takes your source code and a Docker file. 
You'll need to express the building and 
packaging of your source code using a set of instructions in the Docker file. 
Next I'll introduce you to the key Docker file instructions. 
And help you understand what they do. 
Here's a practical application of a Docker file that builds an example, node.js application into a container image. 
This is what it does. 
It starts from a node.js space image. 
It pulls the source code in, then installs dependencies and changes the configuration to run the application when it starts node server.js? 
To understand how this works. It's important to first realize that with Docker you build your application inside the container image. You start with putting a container image on a stage. 
And every doctor file instruction changes that stage container image. 
The general process looks like this. 
You'll start with a base image, which contains tooling to build your application. 
You'll then pull your source code into the container image. 
To build your application you run a program inside of the container image to change files. 
And finally, you'll configure the image to actually start your application. 
Docker files combined the building and packaging of a container image into one process. 
Which results in a few challenges that I'll address later. 

The FROM instruction downloads a base image from a registry and 
puts that on the stage to be modified by later instructions. 

Examples of base images are golang, it has tools to build Go programs. 
And node.js, it has tools to install and build node programs. 

The COPY instructions, pulls in source code. Docker has the concept of a build context, which is usually the directory with the Docker file. 

Use it to bring source code into the stage image you've just downloaded using the FROM instruction. 

The RUN instruction lets you run a program from the image on the image. 
This means the program file you execute needs to be present in the container image. 
And all the files the running program can see are only the files on the container image. 

Examples of tasks you use run for include installing another system package. 
You need to build your application, downloading library dependencies and compiling your source code into binaries. 
Finally, container configuration tells the container runtime such as Docker or Cloud Run. 

What program filed to start from the container image and using what parameters. 
There are several instructions that can change the container configuration. 
Examples include entry point points, to the program file to start, ENV is used to set environment variables. Work DIR sets the working directory of the program, End- User. It sets the user to use when starting the program. This defaults to the system administrator. So this is a setting you don't want to miss. Here's what's important to remember about Docker file instructions. You start with putting a container image on a stage and every doctor file instruction changes that stage container image. 

The FROM instruction downloads a base image to start from. 
The base image can be golang for example and includes tools you need to build and run your software. 

The COPY instruction pulls in files from the build context. Which is usually the directory with the Docker file. 

The RUN instructions lets you run a program from the container image to change files in the image. 

Other instructions change the container configuration, which points out which program filed to start and how. 


Here's what's important to remember about building secure and small container images with Docker files. 
Base images are bloated with packages you don't need for running your application. 
But do need to build your application. 
They contain package managers in Version Control Software. 
It's a risk to add these binaries to the container image you deploy. 
Because they might contain security vulnerabilities. 
Distroless is a project that provides minimal runtime container images. 
These images can't be used to build your application. 
But do contain only a minimal set of system dependencies to run your application. 
If you repeat the FROM instruction, you create a multi stage built. 
And to finish a build, you copy the application and it's dependencies into the final stage.