# NodeJS and npm, As the basic environment for development.
# ----
# build image:
#   docker build --force-rm -t platbase.com/dev.nodejs:latest -t platbase.com/dev.nodejs:1.4-14.17.5 .
# run test:
#   dc-run -

FROM platbase.com/dev.base:latest
LABEL maintainer="https://github.com/platbase"

# The version of node js
ENV NODE_VERSION=14.17.5

ENV DEVOPS_FIXED_ENVS "NODE_VERSION"

# The npm registry server
ENV NPM_REG=https://registry.npm.taobao.org

# Mappings
EXPOSE 8080
VOLUME ["/workspace", "/data", "/tmp/.X11-unix"]

# Entry - force work with user "u01"
CMD /start-nodejs-shell.sh

###############################################################################

# Install NodeJS
RUN wget -O "/tmp/node-v${NODE_VERSION}-linux-x64.tar.xz" https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz && \
    tar xvJf "/tmp/node-v${NODE_VERSION}-linux-x64.tar.xz" -C /opt && \
    chown -Rv u01:u01 /opt/node-v${NODE_VERSION}-linux-x64 && \
    rm "/tmp/node-v${NODE_VERSION}-linux-x64.tar.xz"
RUN export PATH=$PATH:/opt/node-v${NODE_VERSION}-linux-x64/bin&& \
    npm config set prefix /data/nodejs/global --global && \
    npm config set cache  /data/nodejs/cache  --global && \
    npm config set registry ${NPM_REG} --global && \
    echo "export PATH=\$PATH:/opt/node-v${NODE_VERSION}-linux-x64/bin:/opt/node-v${NODE_VERSION}-linux-x64/bin/npm-global/bin" >> /etc/profile

# Add nodejs-init shell scripts
ADD files /
RUN chmod +x /docker-nodejs-init/* && \
    chmod +x /start-nodejs-shell.sh