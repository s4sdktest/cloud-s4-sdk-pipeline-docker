FROM node:11-alpine

ARG npm_registry=https://registry.npmjs.org/
ARG cx_server_base_uri=https://raw.githubusercontent.com/SAP/cloud-s4-sdk-pipeline-docker/master/s4sdk-jenkins-master/cx-server/cx-server

#Install dependencies for running the cx server script in this container
RUN apk add --no-cache bash docker curl && \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*

WORKDIR /cx-server

COPY files/* ./

# Package cx-server(.bat) in this image so we can update them for the user without curl-ing
ADD ${cx_server_base_uri} ${cx_server_base_uri}.bat ./

RUN npm config set registry=$npm_registry&& \
    npm install && \
    npm config delete registry && \
    # If the repository was cloned on Windows, the script might have \CR\LF line endings. Ensure it has only \LF.
    dos2unix cx-server-companion.sh && \
    dos2unix cx-server && \
    chmod +x cx-server cx-server-companion.sh && \
    unix2dos cx-server.bat
