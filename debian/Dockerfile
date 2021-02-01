FROM debian:latest

LABEL maintainer="Masaya Sato <sato.masaya.dev@gmail.com>"
LABEL version="1.0"
LABEL description="The latest Debian distribution for development."

ARG DEBIAN_FRONTEND="noninteractive"

# set the variables as per $(pyenv init -)
ENV LANG="C.UTF-8" \
    LC_ALL="C.UTF-8"

RUN apt-get update && apt-get install -y --no-install-recommends \
        gcc \
        ssh \
        libcurl4-openssl-dev \
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
        ca-certificates \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add a new user
ARG DOCKER_UID=1000
ARG DOCKER_USER=debian
ARG DOCKER_PASSWORD=debian
RUN useradd -m --uid ${DOCKER_UID} --groups sudo ${DOCKER_USER} && \
    echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd

# Verify server certificate
RUN mkdir /usr/local/share/ca-certificates/cacert.org && \
    wget -P /usr/local/share/ca-certificates/cacert.org http://www.cacert.org/certs/root.crt http://www.cacert.org/certs/class3.crt && \
    update-ca-certificates && \
    git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt

# Fetch a collection of config files in the GitHub repo
ARG DOCKER_USER_HOME=/home/debian
RUN mkdir ${DOCKER_USER_HOME}/github
ARG CONFIG_URL=https://github.com/stmsy/config.git
RUN git clone ${CONFIG_URL} ${DOCKER_USER_HOME}/github/config && \
    chown -R ${DOCKER_USER}:${DOCKER_USER} ${DOCKER_USER_HOME}/github

# Enable to show the Git working branch on terminal
RUN mkdir /opt/git && chown ${DOCKER_USER}:${DOCKER_USER} /opt/git
ARG SRC=${DOCKER_USER_HOME}/github/config/git/gitbranch.sh
ARG DEST=/opt/git/gitbranch.sh
RUN ln -s ${SRC} ${DEST}

# Set up .zshrc
ARG SRC=${DOCKER_USER_HOME}/github/config/zsh/docker.zshrc
ARG DEST=${DOCKER_USER_HOME}/.zshrc
RUN ln -s ${SRC} ${DEST} && chown ${DOCKER_USER}:${DOCKER_USER} ${DEST}

# Set up init.el
RUN mkdir ${DOCKER_USER_HOME}/.emacs.d && \
    chown ${DOCKER_USER}:${DOCKER_USER} ${DOCKER_USER_HOME}/.emacs.d
ARG SRC=${DOCKER_USER_HOME}/github/config/emacs/docker.init.el
ARG DEST=${DOCKER_USER_HOME}/.emacs.d/init.el
RUN ln -s ${SRC} ${DEST} && chown ${DOCKER_USER}:${DOCKER_USER} ${DEST}

# Set up custom Emacs packages
ARG ANYTHING_CONFIG_URL=https://repo.or.cz/anything-config.git
ARG ANYTHING_CONFIG_DIRPATH=${DOCKER_USER_HOME}/.emacs.d/site-lisp/anything-config
RUN mkdir -p ${ANYTHING_CONFIG_DIRPATH} && \
    git clone ${ANYTHING_CONFIG_URL} ${ANYTHING_CONFIG_DIRPATH} && \
    chown -R ${DOCKER_USER}:${DOCKER_USER} ${DOCKER_USER_HOME}/.emacs.d/

# Change the login shell to zsh from bash
RUN chsh -s /bin/zsh ${DOCKER_USER}

# Switch user to 'debian'
USER ${DOCKER_USER}

# Set the default working directory
WORKDIR ${DOCKER_USER_HOME}
