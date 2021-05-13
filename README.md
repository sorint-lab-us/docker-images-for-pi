# Docker Images for Raspberry Pi 

The purpose of this project is to provide simple and secure base images of popular 
runtime environments for Raspberry Pi.

Currently there are images for OpenJDK11 (JRE and JDK) , Python, including 
numpy and pandas, and combinations thereof. 

The diagram below shows the relationships between the various images.  The content of each 
images is fairly obvious from the name but the Dockerfiles included as part of this project
provide complete details.

![image hierarchy](./rpi-image-hierarchy.png)

The ancestor of all images in this repository is `sorintlabus/rpi-buster-minbase`.
It was created using the instructions shown below. 

All of the images defined in this project are published on Docker Hub under the "sorintlabus" 
organization.

## Image Sizes

Images sized are shown below.  Currently they are a bit larger than one would like. No 
attempts have been made to slim them down.  Two possible improvements would be: to 
remove unused locales in the base image and to use "jlink" to build a customer JRE that 
does contains a smaller subset of the whole Java runtime. This is an area for future work.

```
REPOSITORY                             TAG       IMAGE ID       CREATED          SIZE
sorintlabus/rpi-pandas-openjdk11-jre   latest    8b3eb19716a8   17 minutes ago   1.01GB
sorintlabus/rpi-pandas                 latest    efbd95b48370   30 minutes ago   847MB
sorintlabus/rpi-numpy-openjdk11-jre    latest    72545d63be7e   2 hours ago      843MB
sorintlabus/rpi-numpy                  latest    67ae996e9e09   2 hours ago      681MB
sorintlabus/rpi-openjdk11-maven        latest    902989b6e92a   3 hours ago      569MB
sorintlabus/rpi-openjdk11-jdk          latest    7a8a644aa28c   3 hours ago      557MB
sorintlabus/rpi-openjdk11-jre          latest    6c3ac3122081   4 hours ago      363MB
sorintlabus/rpi-buster-minbase         latest    83fdc195827b   8 days ago       170MB
```

## Content of the base image: sorintlabus/rpi-buster-minbase

The image was created with debootstrap in as straighforward and simple manner as 
possible.

```
# install docker and debootstrap
# the last 2 are only providing a credential handler for docker login 
# they are not required to build the image but they are needed to 
# push to Docker Hub
sudo apt install docker-compose debootstrap gnupg2 pass 

# create the Buster file system in the "buster-minbase" dir
sudo debootstrap --variant=minbase buster buster-minbase

# install the image locally with the name "rpi-buster-minbase"
sudo tar -C buster-minbase -c . | docker import - rpi-buster-minbase

# test if desired
docker run -it rpi-buster-minbase bash 

# finally, tag it and push it to docker hub
docker login
docker tag rpi-buster-minbase sorintlabus/rpi-buster-minbase
docker push sorintlabus/rpi-buster-minbase

# you may remove the stopped container at this point
```
