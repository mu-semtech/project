# Supporting MacOS (Hello MacOS)
![](http://mu.semte.ch/wp-content/uploads/2017/11/Mac-semtech-1024x768.png)

Many developers use MacOS on their daily basis.  With our Docker background, we’ve mainly focused on Linux support.  That is about to change, MacOS is becoming a first class citizen.

## What does this entail?
Base scripts will receive Docker support, documentation will be verified to run both on Linux as well as on MacOS.  We will supply installation instructions for MacOS.

For most supporting projects we start, we base ourselves on Linux.  The ember docker is a good example of that.  Although Docker abstracts a lot, there are differences.  The scripts for this Docker have received updates already, ensuring they run correctly on MacOS.  By trying out solutions both on Linux as well as on MacOS, we ensure a smooth operating experience.

We are looking into ways of supplying code for MacOS so the installation is simplified.  For the backend, installation is trivial with Docker for Mac.  For the frontend, we’re considering to support homebrew installation of edi.

## What differences are considered?
Bigger differences for us come in the form of filesystem performance, supplied shell scripts, and user namespaces.

Filesystem performance for mounted volumes is an issue for MacOS.  Both starting of containers, as well as syncing content between the container and the host seems to be an order of magnitude slower.  We are updating scripts to ensure the system works as expected.  Performance improvements are currently only noticeable on MacOS.

With Linux we target a modern kernel and a modern Bash environment.  The shell scripts Linux ships with are slightly different than those MacOS has in store.  This means some scripts need to be adapted. Some will become cleaner, some will become more complex.

User Namespaces ensure we can generate files from Docker, with your access rights.  This alleviates the issue where you need to chown generated files.  Docker for Mac doesn’t have that issue.  It does not run as root, hence the containers generate files under your own username.

## In conclusion
If your organisation is a cross-breed between Linux and MacOS, it will become easier to get everyone on board with mu.semte.ch.  We’ll keep you posted as we hum along.

*This document has been adapted from Aad Versteden's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/11/09/hello-macos/)*
