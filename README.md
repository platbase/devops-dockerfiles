# devops-dockerfiles

The Dockerfiles for development, testing and deployment.

## Abstract

- `develop-tools` - Tools(always shell scripts) to run images
- `devenv` - for development
  - `00-base` - The basic docker images
  - `10-devenv` - The docker images for variants of development environment
  - `@demos` - Demonstrations for variants of development environment
- `servers` - server environments
  - `00-base` - The basic docker images
  - `10-database` - The docker images for variants of database environment

## Tips

#### Build image with proxy

In case when use `docker0` network (The bridge network created automatically when install Docker Engine), and run HTTP/HTTPS proxy server in host OS with port `8123`:

```bash
# Get the IP address of docker0 network
export DOCKER0_IP=$(ip route | grep docker0 | awk '{print $9}')

# Build arguments for proxy
export PROXY="--build-arg http_proxy=http://$DOCKER0_IP:8123 --build-arg https_proxy=http://$DOCKER0_IP:8123"

# Use proxy build arguments to build image, for example
docker build $PROXY --force-rm -t platbase.com/dev.base:1.4-20.04 .

# ======== Tested in Ubuntu 20.04 ========
```

### TODO

1. 
2. cp -Rv /home/u01 /data/home 在存在 /data/home 的情况下，会多一层 u01.
