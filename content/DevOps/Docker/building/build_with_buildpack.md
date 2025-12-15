After a tour of Dockerfiles, it's now time
to explore another option for building container images Buildpacks, Buildpacks
provide a hands off experience and turn your source code into
a container image on autopilot. Buildpacks are a way to turn source code
into a container image without writing a Dockerfile. Buildpacks are built into Cloud Run
to enable the source based deployment workflow. In short, Buildpacks provide developers with
a convenient way to work with container images without thinking about the
complexities that come with building them. Buildpacks is an open source project and
it's plugable, multiple vendors offer Buildpacks. You can create your own Buildpacks and you can inspect the Buildpacks
that Cloud Run uses internally. Let's start with the visual model of
what Buildpacks are and what they do. A builder shown in the middle is
the central and most important unit, it turns your source code
into a container image. A builder can support multiple
types of source code. In this example, the builder can build and
package a Python, Node.js, Go and
a Java application into a container image. Every builder contains
a collection of Buildpacks, the little robots in the diagram. The Buildpacks do the actual work to
build and package the container image. If a builder starts to process a source
directly, it executes two phases. The detect phase where the builder asks
all Buildpacks if they can process the source and the build phase where
all builders that think they can contribute are activated and
perform their part. For example, an npm Buildpack will look
for a package.chase on file in the source. If it finds one, it will activate and perform an npm install
during the build phase. Let's look at that in more detail, after
the builder runs the detect phase and asks all Buildpacks to contribute,
only suitable Buildpacks activate and perform a build while others do not. In this example, three Buildpacks
activate to build a directory with a Javascript project,
one Buildpack installs a Node.js runtime. Another build pack runs npm install,
and the final Buildpack configures the resulting image to
start the Node.js runtime. The result is a container image,
you can deploy to Cloud Run or run with Docker locally. I started with explaining
the intervals of the builder because you need to have a conceptual
understanding of how it works. However, as a developer you're
not exposed to those internals. In fact, from the developer perspective,
one of the best parts of Buildpacks is the ease of
use with the command line tool pack. You can use a builder, any builder to
turn source code into a container image. In this example, you see how to use
pack to build a source directory using the Google Cloud Buildpacks builder. That's the builder that's built into
Cloud Run, the example shows that you can reproduce on your local machine what Cloud
Run does to build your app in the Cloud. There are multiple projects out there that
use the Buildpack standards to create their own builders. This means you can choose any builder,
examples include Paketo Buildpacks. This is a Cloud foundry foundation project
which is dedicated to maintaining a vendor neutral governance. Heroku Buildpacks, Heroku is
a well known platform as a service product that makes it easy to
build Cloud native applications. Internally, it uses Heroku Buildpacks and
they're open source. Regardless of what Buildspack
you used to build your container Image, Cloud Run can run it. Google Cloud Buildpacks
are built into Cloud Run. You can deploy your source code as
well as container images to Cloud Run. In fact, Google Cloud Buildpacks
are used internally by App Engine, Cloud Functions and Cloud Run. The Google Cloud Buildpacks
builder supports, Go, Java, Node.js, Python and
.NET Core out of the box. Google Cloud Buildpacks is an open source
project, which means that users can, with approval contribute to the project. Deploying source code without
previously containerising it is possible with Cloud Run
because Cloud Buildpacks will be triggered to build the application
from this source code for you.