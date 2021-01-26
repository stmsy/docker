FROM ubuntu:18.04

MAINTAINER Masaya Sato <sato.masaya.dev@gmail.com>

ARG DEBIAN_FRONTEND="noninteractive"

# Set the variables as per $(pyenv init -)
ENV LANG="C.UTF-8" \
    LC_ALL="C.UTF-8" \
    PATH="/opt/pyenv/shims:/opt/pyenv/bin:$PATH" \
    PYENV_ROOT="/opt/pyenv" \
    PYENV_SHELL="bash"

# Install the required libraries from APT repo
RUN echo 'deb http://security.ubuntu.com/ubuntu bionic-security main' >> /etc/apt/sources.list
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        ca-certificates \
        curl \
        git \
        libbz2-dev \
        libffi-dev \
        libncurses5-dev \
        libncursesw5-dev \
        libreadline-dev \
        libsqlite3-dev \
        libssl1.0-dev \
        liblzma-dev \
        llvm \
        make \
        netbase \
        pkg-config \
        tk-dev \
        wget \
        xz-utils \
        zlib1g-dev \
        fonts-ipafont && \
   rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install pyenv
COPY pyenv_version.txt python_version.txt /
RUN git clone -b `cat /pyenv_version.txt` --single-branch --depth 1 https://github.com/pyenv/pyenv.git $PYENV_ROOT && \
    for version in `cat /python_version.txt`; do pyenv install $version; done && \
    pyenv global `cat /python_version.txt` && \
    rm -rf /tmp/* && \
    rm pyenv_version.txt python_version.txt

# Create Python virtual env for Facebook Prophet
RUN conda update -y -n base -c defaults conda && \
    conda install -y -c conda-forge --repodata-fn=repodata.json nodejs && \
    conda install -y -c conda-forge fbprophet dask distributed && \
    conda install -y -c plotly plotly && \
    pip install -U \
        jupyterlab \
	jupyter-contrib-core \
	jupyter-nbextensions-configurator \
        jupyterlab-git \
        xlrd
RUN jupyter labextension install jupyterlab-emacskeys
RUN jupyter labextension install @jupyterlab/katex-extension
RUN jupyter labextension install @jupyterlab/toc

# Set the default working directory
WORKDIR /root/notebooks
