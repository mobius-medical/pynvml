ARG CUDA_VERSION=9.1
ARG UBUNTU_VERSION=16.04
FROM nvidia/cuda:${CUDA_VERSION}-devel-ubuntu${UBUNTU_VERSION} AS build-stage

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    build-essential \
    curl \
    libbz2-dev \
    libffi-dev \
    liblzma-dev \
    libncurses5-dev \
    libpython2.7-dev \
    libreadline6-dev \
    libsqlite3-dev \
    libssl-dev \
    make \
    openssh-client \
    python \
    tk-dev \
    zlib1g-dev \
  && rm -rf /var/lib/apt/lists/*

# Install Python 3.7 and tox
ADD https://www.python.org/ftp/python/3.7.13/Python-3.7.13.tgz Python-3.7.13.tgz
RUN tar xf Python-3.7.13.tgz && cd Python-3.7.13/ && ./configure && make && make install
RUN pip3 install tox

ARG DOCKER_USER=jenkins
ARG DOCKER_UID=1002
ARG DOCKER_GROUP=jenkins
ARG DOCKER_GID=1001
RUN groupadd --gid $DOCKER_GID $DOCKER_GROUP \
    && useradd --uid $DOCKER_UID --gid $DOCKER_GID --shell /bin/bash --create-home $DOCKER_USER

RUN chown -R $DOCKER_USER:$DOCKER_USER /home/$DOCKER_USER

USER $DOCKER_USER

RUN mkdir -p -m 0700 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
ENV PATH=/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}

ENTRYPOINT bash
