# The basic environment for development
# ----
# build image:
#   docker build --force-rm -t platbase.com/dev.base:1.2 .
# run:
#   docker run -it --rm -v /tmp/dev-base.ws:/workspace -v /tmp/dev-base.data:/data platbase.com/dev.base:1.2

FROM ubuntu:16.04
MAINTAINER https://github.com/platbase

# Mappings
VOLUME ["/workspace", "/data"]

# Entry - force work with user "u01"
CMD /start-example.sh

###############################################################################

# Preparing installation and install basic tools
ADD files/sources.list /etc/apt/sources.list
RUN apt-get update
RUN \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get install -y --allow-unauthenticated sudo; \
    apt-get install -y --allow-unauthenticated wget;

# Install necessary packages
RUN \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get install -y --allow-unauthenticated net-tools; \
    apt-get install -y --allow-unauthenticated inetutils-ping; \
    apt-get install -y --allow-unauthenticated python; \
    apt-get install -y --allow-unauthenticated build-essential
RUN \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get install -y --allow-unauthenticated telnet; \
    apt-get install -y --allow-unauthenticated nano

# Install and prepare cron
RUN \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get install -y --allow-unauthenticated cron
RUN touch /root/.crontab

# Add work user
RUN useradd -mU -u 1000 -d /home/u01 -s /bin/bash u01
RUN echo "u01:docker.io" | chpasswd     # Can't set password properly with "-p" argument in "useradd" command
RUN usermod -a -G sudo u01

# Prepare shell environment
RUN touch /home/u01/.bashrc-docker-PATH
RUN touch /home/u01/.bashrc-docker-PS1
RUN touch /home/u01/.bashrc-docker-CMD
RUN echo "# Added by platbase.com/dev.base"         >> /home/u01/.bashrc
RUN echo ". ./.bashrc-docker-PATH"                  >> /home/u01/.bashrc
RUN echo ". ./.bashrc-docker-PS1"                   >> /home/u01/.bashrc
RUN echo ". ./.bashrc-docker-CMD"                   >> /home/u01/.bashrc

# Force u01 the owner of /workspace and /data
RUN chown -R u01:u01 /workspace
RUN chown -R u01:u01 /data

# Add base-init shell scripts
ADD files/docker-base-init /docker-base-init
RUN chmod +x /docker-base-init/*

# Prepare starting commands
ADD files/start-example.sh /start-example.sh
RUN chmod +x /start-example.sh

###############################################################################

# Environment variable
#  - To mark the environment variables which should be used in "u01"'s shell.
#  - because: su option "-, -l, --login" should build a new environment(see option "-m, -p, --preserve-environment")
#  - NOTE: default value "TERM LANG DISPLAY" was assumed in shell script "docker-boot"
ENV DEVOPS_FIXED_ENVS ""
