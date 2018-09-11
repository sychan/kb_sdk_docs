
Dependencies & Prerequisites
==================================

System Dependencies:

* Mac OS X 10.8+ (Docker requires this) or Linux.  kb-sdk does not run natively in Windows
* (Not required for Docker-based installation) Java JRE 7 or 8 (9 is currently incompatible) http://www.oracle.com/technetwork/java/javase/downloads/index.html
* (Mac only) Xcode https://developer.apple.com/xcode
* git https://git-scm.com
* Docker https://www.docker.com (for local testing)
* At least 20 GB free on your hard drive to install Docker, Xcode, Java JRE, etc.

We recommend using Mac OS X 10.8+ or Linux for SDK development. Windows development is not currently supported.  If you are running Windows or do not want to develop on your local machine, we recommend using [VirtualBox](https://www.virtualbox.org) and installing Ubuntu 14+.

Xcode or the Xcode Command Line Tools will install some standard terminal commands like `make` and `git` that are necessary for building the KBase SDK and your module code. https://developer.apple.com/xcode

The git homepage can be found at https://git-scm.com. On Ubuntu, install it with ``sudo apt-get install git``.

Docker Installation
---------------------

KBase module code is run using Docker, which allows you to easily install all system tools and dependencies your module requires. Installing Docker locally allows you to test your build and run tests on your own computer before registering your module with KBase.

Instructions for installing on Mac: https://www.docker.com/mac

Instructions for installing on Linux: https://www.docker.com/linux

On Linux, be sure to follow these `post installation steps <https://docs.docker.com/install/linux/linux-postinstall/>`_ so you can run docker and `kb-sdk` as a non-root user.

When you run an app in a narrative, it runs in a docker container on KBase's servers. Learn more about docker containers here: https://www.docker.com/what-container. Docker containers allow you to package and compile programs and dependencies, such as `MEGAHIT <https://github.com/voutcn/megahit>`_, and run them anywhere.

