FROM debian:latest

MAINTAINER Masaya Sato <sato.masaya.dev@gmail.com>

ARG DEBIAN_FRONTEND="noninteractive"

# set the variables as per $(pyenv init -)
ENV LANG="C.UTF-8" \
    LC_ALL="C.UTF-8"

RUN apt-get update && apt-get install -y --no-install-recommends \
        gcc \
        ssh \
        openssl \
        sudo \
        emacs \
        zsh \
        git \
        make \
        build-essential \
        libssl-dev \
        zlib1g-dev \
        libbz2-dev \
        libreadline-dev \
        libsqlite3-dev \
        wget \
        curl \
        llvm \
        libncurses5-dev \
        libncursesw5-dev \
        xz-utils \
        tk-dev \
        libffi-dev \
        liblzma-dev \
        python-openssl \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ARG DOCKER_UID=1000
ARG DOCKER_USER=debian
ARG DOCKER_PASSWORD=debian
RUN useradd -m --uid ${DOCKER_UID} --groups sudo ${DOCKER_USER} \
	&& echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd

COPY config/ssl/ca-certificates.crt /etc/ssl/certs
RUN cat /etc/ssl/certs/ca-certificates.crt

# Fetch a collection of config files located at the GitHub repo
ARG DOCKER_USER_HOME=/home/debian
RUN mkdir ${DOCKER_USER_HOME}/github
RUN git clone https://github.com/stmsy/config.git ${DOCKER_USER_HOME}/github/config
RUN chown -R ${DOCKER_USER}:${DOCKER_USER} ${DOCKER_USER_HOME}/github

# Enable to show the Git working branch
RUN mkdir /opt/git
RUN chown ${DOCKER_USER}:${DOCKER_USER} /opt/git
ARG SRC=${DOCKER_USER_HOME}/github/config/git/gitbranch.sh
ARG DEST=/opt/git/gitbranch.sh
RUN ln -s ${SRC} ${DEST}

# Set up .zshrc
ARG SRC=${DOCKER_USER_HOME}/github/config/zsh/docker.zshrc
ARG DEST=${DOCKER_USER_HOME}/.zshrc
RUN ln -s ${SRC} ${DEST}
RUN chown ${DOCKER_USER}:${DOCKER_USER} ${DEST}

# Set up init.el
RUN mkdir ${DOCKER_USER_HOME}/.emacs.d
RUN chown ${DOCKER_USER}:${DOCKER_USER} ${DOCKER_USER_HOME}/.emacs.d
ARG SRC=${DOCKER_USER_HOME}/github/config/emacs/docker.init.el
ARG DEST=${DOCKER_USER_HOME}/.emacs.d/init.el
RUN ln -s ${SRC} ${DEST}
RUN chown ${DOCKER_USER}:${DOCKER_USER} ${DEST}

# Set up custom Emacs packages
RUN mkdir -p ${DOCKER_USER_HOME}/.emacs.d/site-lisp/anything-config
RUN git clone https://repo.or.cz/anything-config.git ${DOCKER_USER_HOME}/.emacs.d/site-lisp/anything-config
run chown -R ${DOCKER_USER}:${DOCKER_USER} ${DOCKER_USER_HOME}/.emacs.d/

# Change the login shell to zsh from bash
RUN chsh -s /bin/zsh ${DOCKER_USER}

# Switch user to 'debian'
USER ${DOCKER_USER}

WORKDIR ${DOCKER_USER_HOME}
