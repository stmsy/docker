FROM jupyter/datascience-notebook:lab-2.2.9

LABEL maintainer="Masaya Sato <sato.masaya.dev@gmail.com>"
LABEL version="1.0"
LABEL description="The JupyterLab image for data science projects."

ARG DOCKER_USER=jovyan
ARG DOCKER_USER_HOME=/home/${DOCKER_USER}
RUN rm -rf ${DOCKER_USER_HOME}/work
COPY config/ ${DOCKER_USER_HOME}/config/

RUN pip install -r ${DOCKER_USER_HOME}/config/python/requirements.txt
