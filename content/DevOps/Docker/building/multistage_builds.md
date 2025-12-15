# Multi-stage builds #2: Python specifics

Your Python code needs some compilation, and you’ve learned that [multi-stage builds are the way to get smaller Docker images](https://pythonspeed.com/articles/smaller-python-docker-images/). But how exactly do multi-stage images work, in general and for Python? How should you implement them in practice?

In this article you’ll learn:

1. The basics of how multi-stage builds work, and how Python makes them a little harder.
2. Solving the problem with `pip install --user`.
3. A `virtualenv`-based solution.
4. Some additional solutions: building wheels, pex, and platter.

## Multi-stage Docker builds

As I covered in [more detail elsewhere](https://pythonspeed.com/articles/smaller-python-docker-images/), installing a compiler in one part of the Docker file makes the resulting image much larger. If you want smaller images, the best solution is usually a multi-stage Docker build.

The simplest case is a compiler image and a runtime image: the compiler image compiles the code, and the runtime image copies the resulting artifact over. The runtime image never includes the compiler layer, and can therefore be smaller in size.

Let’s see how this looks in practice:

```
# This is the first image:
FROM ubuntu:18.04 AS compile-image
RUN apt-get update
RUN apt-get install -y --no-install-recommends gcc build-essential

WORKDIR /root
COPY hello.c .
RUN gcc -o helloworld hello.c

# This is the second and final image; it copies the compiled
# binary over but starts from the base ubuntu:18.04 image.
FROM ubuntu:18.04 AS runtime-image

COPY --from=compile-image /root/helloworld .
CMD ["./helloworld"]
```


If we do `docker build` on the above, the final image is the last stage, the `runtime-image`.

As a result the image is only 88.9MB, basically the same size as the `ubuntu:18.04` it builds on (and with which it shares most of its layers): we have an image that gets the compiled artifact without having to include a compiler in its layers.

## The problem with Python and multi-stage builds

Let’s assume you have a relatively complex Python project that has some code (in C/C++/Rust) that needs compiling, as well as a number of Python dependencies.

Not all of your dependencies will have been packaged as wheels (precompiled Python packages). So possibly your dependencies will also need a compiler to be installed.

In the example above we copied a single binary, which made life easier: just one thing to copy. But Python packages install many things in many places—if you just do a normal `pip install`, you will end up with:

- Code and other files (e.g. `.pth` files) installed in your Python install’s `site-packages`.
- Scripts installed in `/usr/bin`.
- Data files installed somewhere else again, usually under `/usr`.

In the general case, figuring out all the files you need to copy from the compiler stage to the runtime stage can be difficult, and the resulting configuration will be brittle.

## Solution #1: pip install –user

If you install a package with `pip`’s `--user` option, all its files will be installed in the `.local` directory of the current user’s home directory. That means all the files will end up in a single place you can easily copy.

That is, assuming your application is pip installable, all you need to do is copy `.local`. If it isn’t you additionally just copy the directory your code is in.

```
FROM python:3.7-slim AS compile-image
RUN apt-get update
RUN apt-get install -y --no-install-recommends build-essential gcc

COPY requirements.txt .
RUN pip install --user -r requirements.txt

COPY setup.py .
COPY myapp/ .
RUN pip install --user .

FROM python:3.7-slim AS build-image
COPY --from=compile-image /root/.local /root/.local

# Make sure scripts in .local are usable:
ENV PATH=/root/.local/bin:$PATH
CMD ['myapp']
```

The main downside to this approach is that you are still sharing your Python install with any system Python packages that [might exist in one of your system-level dependencies](https://hynek.me/articles/virtualenv-lives/). If that affects your application, the other solution is to use a virtualenv.

## Solution #2: virtualenv

A virtualenv is another way to get a single isolated directory you can copy over into your final Docker image (see [this explanation of how I’m activating the virtualenv](https://pythonspeed.com/articles/activate-virtualenv-dockerfile/)):

```
FROM python:3.7-slim AS compile-image
RUN apt-get update
RUN apt-get install -y --no-install-recommends build-essential gcc

RUN python -m venv /opt/venv
# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY setup.py .
COPY myapp/ .
RUN pip install .

FROM python:3.7-slim AS build-image
COPY --from=compile-image /opt/venv /opt/venv

# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"
CMD ['myapp']
```

## Other solutions: Wheels, pex, platter

Another solution to the problem is to build all the packages as wheels—binary packages with the compiled code—and then copy the wheel files over to the build stage and install them there. This works, but doesn’t really add much.

You can also use tools like [pex](https://pex.readthedocs.io/en/stable/) or [platter](http://platter.pocoo.org/) to create a single file you can copy. Pex will create a single-file executable (which means it won’t work in some edge cases) and Platter will basically untar into a virtualenv. Again, it’s not clear why this is better than the methods discussed above.

## Faster multi-stage builds

While the proposed methods above will result in much smaller images, if you’re not careful you can end up with slow builds: the default of way of doing caching in Docker doesn’t work quite right with multi-stage builds. So you may want to read my guide to [faster multi-stage builds](https://pythonspeed.com/articles/faster-multi-stage-builds/).