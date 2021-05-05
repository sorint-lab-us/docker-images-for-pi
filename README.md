# Docker Images for Raspberry Pi 

All images in this repository derive from a Debian buster parent image that was created
using the instructions shown below.  The parent image is designed to have a very simple 
provenance to promote confidence.  

The instructions for creating the parent image are below.  All steps are executed on a 
Raspberry Pi with internet access. [Debootstrap](https://wiki.debian.org/Debootstrap) 
is used to download the image artifacts from the official Debian buster stable 
repository.

```
# install docker and debootstrap
# the last 2 are only providing a credential handler for docker login 
# they are not required to build the image but they are needed to 
# push to Docker Hub
sudo apt install docker-compose debootstrap gnupg2 pass 

# create the Buster file system in the "buster-minbase" dir
sudo debootstrap --variant=minbase buster buster-minbase

# install the image locally with the name "buster-minbase"
sudo tar -C buster-minbase -c . | docker import - buster-minbase

# test if desired
docker run -it buster-minbase bash 

# finally, tag it and push it to docker hub
docker login
docker tag buster-minbase wrmay/buster-minbase
docker push wrmay/buster-minbase

# you may remove the stopped container at this point
```
