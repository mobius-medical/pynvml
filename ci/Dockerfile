# syntax=docker/dockerfile:1.4
# pynvml as distributed on pypi requires python 3.6+
# This Dockerfile builds and tests for python 2.7
#
# Supports Ubuntu 16.04 and 20.04

ARG UBUNTU_BASE_TAG=20.04
FROM ubuntu:${UBUNTU_BASE_TAG} as pynvml-build
LABEL description="Build image for pynvml"

ARG DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-c"]

ENV PACKAGES_PYENV \
  make build-essential libssl-dev zlib1g-dev \
  libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
  libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev


RUN apt-get update \
    && apt-get install -y \
        curl \
        git \
        $PACKAGES_PYENV \
    && apt-get clean

ARG PY2_VERSION=2.7.18
ARG PY3_VERSION=3.7.14

ENV HOME=/root
ENV PYENV_ROOT=$HOME/.pyenv
ENV PATH=$PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH
RUN curl https://pyenv.run | bash
RUN pyenv install $PY2_VERSION
RUN pyenv install $PY3_VERSION
RUN pyenv global $PY2_VERSION $PY3_VERSION  && python3 -m pip install tox && pyenv rehash

ADD . /sandbox
WORKDIR /sandbox

# build wheels during docker build
RUN tox -p -e '{py27,py37}'-wheel

# test with `docker run`
# requires gpu which not available during build
CMD tox -e py27,py37
