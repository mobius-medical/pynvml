# syntax=docker/dockerfile:1.4
# pynvml as distributed on pypi requires python 3.6+
# This Dockerfile builds and tests for python 2.7
#
# Supports Ubuntu 16.04 and 20.04

ARG UBUNTU_BASE_TAG=16.04
FROM ubuntu:${UBUNTU_BASE_TAG} as pynvml-build
LABEL description="Build image for pynvml"

ARG DEBIAN_FRONTEND=noninteractive

ENV PY3_DEPS \
  make build-essential libssl-dev zlib1g-dev \
  libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
  libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev


RUN apt-get update \
    && apt-get install -y \
        curl \
        git \
        python \
        $PY3_DEPS \
    && apt-get clean

# Install Python 3.7 and tox
ADD https://www.python.org/ftp/python/3.7.13/Python-3.7.13.tgz Python-3.7.13.tgz
RUN tar xf Python-3.7.13.tgz && cd Python-3.7.13/ && ./configure && make -j$(nproc) && make install
RUN pip3 install tox
ENV TOX_PARALLEL_NO_SPINNER=1

ARG DOCKER_USER=jenkins
ARG DOCKER_UID=1002
ARG DOCKER_GROUP=jenkins
ARG DOCKER_GID=1001
RUN groupadd --gid $DOCKER_GID $DOCKER_GROUP \
    && useradd --uid $DOCKER_UID --gid $DOCKER_GID --shell /bin/bash --create-home $DOCKER_USER

USER $DOCKER_USER

CMD /bin/bash
