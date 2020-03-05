FROM jenkins/jnlp-slave:latest

ARG DEBIAN_FRONTEND=noninteractive

#timezone
ENV TZ=America/Toronto

USER root

RUN apt-get update && \
    apt-get install -qq -y --no-install-recommends \
      apt-transport-https \
      ca-certificates \
      curl \
      sudo \
      gnupg2 \
      software-properties-common

# install docker & docker-compose
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add - \
    && apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

RUN apt-get update \
    && apt-get install docker-ce -y

RUN if [ -z ${COMPOSE_RELEASE+x} ]; then \
        COMPOSE_RELEASE=$(curl -sX GET "https://api.github.com/repos/docker/compose/releases/latest" \
        | awk '/tag_name/{print $4;exit}' FS='[""]'); \
 fi && \
 curl -L \
 "https://github.com/docker/compose/releases/download/${COMPOSE_RELEASE}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Setup timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata
