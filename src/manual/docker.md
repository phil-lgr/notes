# Docker

```bash
# remove all images
docker rmi $(docker images -a -q)
docker images -q | %{docker rmi -f $_}

# remove all exited containers
docker rm $(docker ps -a -f status=exited -q)

# stop and remove all containers
docker rm $(docker ps -a -q)

# create image from container
docker commit NAME flight212121/repo:tag
```
