# web-helloworld-c
![](https://img.shields.io/github/license/open-horizon-services/web-helloworld-c)
![](https://img.shields.io/badge/architecture-arm32-green)
![](https://img.shields.io/github/contributors/open-horizon-services/web-helloworld-c)

Extremely simple HTTP server (written in C) that responds on port 8000 with a hello message. The docker container is built using the "multi-stage build process, with the second build stage being `FROM scratch` (a completely empty file system with no Linux distro). For details on how to do that, see the Dockerfile.

I think this will build on many hardware architectures. :-)  I tested in on a Raspberry Pi 3B (arm32v7) and the image size ended up being 0.4MB (see below). That's pretty tiny considering that the extremely tiny `alpine` Linux distro base container with no workload inside at all is about 10 times larger than this:

```
-rw-r--r-- 1 pi pi 4098560 Apr 27 20:03 alpine.tar
-rw-r--r-- 1 pi pi  419840 Apr 28 14:39 web-hello-c.tar
```

## Prerequisites

**Management Hub**: [Install the Open Horizon Management Hub](https://open-horizon.github.io/quick-start) or have access to an existing hub in order to publish this service and register your edge node.  You may also choose to use a downstream commercial distribution based on Open Horizon, such as IBM's Edge Application Manager.  If you'd like to use the Open Horizon community hub, you may [apply for a temporary account](https://wiki.lfedge.org/display/LE/Open+Horizon+Management+Hub+Developer+Instance) and have credentials sent to you.

**Docker**: [Install Docker](https://docs.docker.com/install/) on your development machine. You will need to be able to run Docker commands as a non-root user.  If you are using Linux, you can [add your user to the docker group](https://docs.docker.com/install/linux/linux-postinstall/).  If you are using Mac or Windows, you can [install Docker Desktop](https://www.docker.com/products/docker-desktop) and use the Docker Desktop UI to run Docker commands.

**Optional utilities to install:**  With `brew` on macOS (you may need to install _that_ as well), `apt-get` on Ubuntu or Raspberry Pi OS, `yum` on Fedora, install `gcc`, `make`, `git`, `jq`, `curl`, `net-tools`.  Not all of those may exist on all platforms, and some may already be installed.  But reflexively installing those has proven helpful in having the right tools available when you need them.

## Installation
clone this repo and cd into the directory
NOTE: This assumes that `git` has been installed.
``` shell
  git clone https://github.com/open-horizon-services/web-helloworld-c
  cd web-helloworld-c
  ```  ```

Run `make clean` to confirm that the "make" utility is installed and working.

Confirm that you have the Open Horizon agent installed by using the CLI to check the version:

  ``` shell
  hzn version
  ```

  It should return values for both the CLI and the Agent (actual version numbers may vary from those shown):

  ``` text
  Horizon CLI version: 2.30.0-744
  Horizon Agent version: 2.30.0-744
  ```

  If it returns "Command not found", then the Open Horizon agent is not installed.


## Usage

To manually run this container locally as a test, enter `make run`.  This will start the container. Enter `make browse` to open a browser to `localhost:8000`.  When you are done, run `make stop` in the terminal to end the test.

To create [the service definition](https://github.com/open-horizon/examples/blob/master/edge/services/helloworld/CreateService.md#build-publish-your-hw), publish it to the hub, and then form an agreement to download and run Node-RED, enter `make publish`.  When installation is complete and an agreement has been formed, exit the watch command with Control-C.  You may then open a browser pointing to Node-RED by entering `make browse` or visiting [http://localhost:8000/](http://localhost:8000/) in a web browser.


## Advanced details
You can change the desired port the container is opened on by changing the `#define WEB_SERVER_PORT 8000` variable in the webhello.c file, and also changing it on the service.json.  You can also change the `SERVICE_NAME` variable to change the name of the service.  The `SERVICE_NAME` variable is used in the `publish` target of the Makefile.

### Authors
* [John Walicki](https://github.com/johnwalicki)
* [Troy Fine](https://github.com/t-fine)
