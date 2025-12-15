# Image optimization

https://pythonspeed.com/articles/smaller-docker-images/

## Change python image
- Safest choice (when iyou don't know what to put): full official image
	`python:3.10.6`
- -slim
	image with the minimum packages to run python
	test thoroughly when using this and when something breaks see if using the full image resolves it
- -alpine
	https://dev.to/asyazwan/moving-away-from-alpine-30n4
	https://medium.com/@stschindler/the-problem-with-docker-and-alpines-package-pinning-18346593e891

### Guidelines
-   If I need to get something up and running quickly, have no space constraints, and don’t have time to test much, I start with the de-facto image. My main concern here is that the image has everything I need to work out of the box, and I don’t have to worry much. This image, however, will be the largest.
-   If space is a concern and I know I need only the minimal packages for running a particular language like python, I go with -slim.
-   For some projects that I have time to test thoroughly, and have extreme space constraints, I will use the Alpine images. But be aware that this can lead to longer build times and obscure bugs. If you are having trouble porting your docker containers to new environments, or something breaks as you add new packages, it may be because of the Alpine image.
-   Lastly, always scroll to the bottom of the [DockerHub page](https://hub.docker.com/_/python) for a particular image and read about suggestions for choosing an image.
### Comparing docker image sizes
If you want to inspect docker images yourself and compare, try this.
```
docker pull --quiet python:3.8  
docker pull --quiet python:3.8.3  
docker pull --quiet python:3.8.3-slim  
docker pull --quiet python:3.8.3-alpine  
docker images
```


# Use as non-root user

```
# create docker group
sudo groupadd docker

# add user to docker group
sudo usermod -aG docker $USER 

# activate changes to the group
newgrp docker

# Change the permissions of docker socket to be able to connect to the docker daemon `/var/run/docker.sock`
sudo chown $USER /var/run/docker.sock
```

### Error: permission denied
error:
	WARNING: Error loading config file: /home/user/.docker/config.json -
	stat /home/user/.docker/config.json: permission denied
fix:
```
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```