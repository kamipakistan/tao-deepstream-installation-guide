# Install Docker Engine on Ubuntu
To get started with Docker Engine on Ubuntu, make sure you meet the prerequisites, and then follow the installation steps.

## Prerequisites
### OS requirements
To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:
* Ubuntu Kinetic 22.
* Ubuntu Jammy 22.04 (LTS)
* Ubuntu Focal 20.04 (LTS)
* Ubuntu Bionic 18.04 (LTS)

Docker Engine is compatible with x86_64 (or amd64), armhf, arm64, and s390x architectures.

## Uninstall old versions
Older versions of Docker went by the names of `docker`, `docker.io`, or `docker-engine`, you might also have installations of `containerd` or `runc`. Uninstall any such older versions before attempting to install a new version:

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
`apt-get` might report that you have none of these packages installed.

> Images, containers, volumes, and networks stored in `/var/lib/docker/` aren’t automatically removed when you uninstall Docker. If you want to start with a clean installation, and prefer to clean up any existing data follow the following.
1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:
    ```
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
    ```
2. Images, containers, volumes, or custom configuration files on your host aren’t automatically removed. To delete all images, containers, and volumes:
    ```
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
    ```

# Installation methods

You can install Docker Engine in different ways, depending on your needs:
1. Docker Engine comes bundled with Docker Desktop for Linux. This is the easiest and quickest way to get started.
2. Set up and install Docker Engine from Docker’s apt repository.
3. Install it manually and manage upgrades manually.
4. Use a convenience scripts. Only recommended for testing and development environments.


## Install using the apt repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Set up the repository
1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:
    ```
    sudo apt-get update
    sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
    ```
    
2. Add Docker’s official GPG key:
    ```
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    ```
3. Use the following command to set up the repository:
    ```
    echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

### Install Docker Engine
1. Update the apt package index:
    ```
     sudo apt-get update
    ```
2. Install Docker Engine, containerd, and Docker Compose.
To install the latest version, run:
    ```
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
    > To install a specific version of Docker Engine, start by listing the available versions in the repository:
    
    ```
    # List the available versions:
    apt-cache madison docker-ce | awk '{ print $3 }'
    
    5:20.10.16~3-0~ubuntu-jammy
    5:20.10.15~3-0~ubuntu-jammy
    5:20.10.14~3-0~ubuntu-jammy
    5:20.10.13~3-0~ubuntu-jammy
    ```
    Select the desired version and install:
    ```
    VERSION_STRING=5:20.10.13~3-0~ubuntu-jammy
    sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
    ```
    
3. Verify that the Docker Engine installation is successful by running the hello-world image:
    ```
    sudo docker run hello-world
    ```
    This command downloads a test image and runs it in a container. When the container runs, it prints the following confirmation message and exits.
        
    ```
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    2db29710123e: Pull complete 
    Digest: sha256:4e83453afed1b4fa1a3500525091dbfca6ce1e66903fd4c01ff015dbcb1ba33e
    Status: Downloaded newer image for hello-world:latest
    
    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    
    To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.
    
    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash
    
    Share images, automate workflows, and more with a free Docker ID:
     https://hub.docker.com/
    
    For more examples and ideas, visit:
     https://docs.docker.com/get-started/
    ```
    
### Post-installion after Docker

1. Create the docker group.
    ```
    sudo groupadd docker
    ```
2. Add your user to the docker group.
    ```
    sudo usermod -aG docker $USER
    ```
3. You can also run the following command to activate the changes to groups:
    ```
    newgrp docker
    ```
4. Verify that you can run docker commands without sudo.
    ```
    docker run hello-world
    ```
    if you got error follow official [link of post installation](https://docs.docker.com/engine/install/linux-postinstall/).
    