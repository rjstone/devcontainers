# syntax=docker/dockerfile:1
# for format details, see https://docs.docker.com/engine/reference/builder/

ARG VARIANT="noble"

ARG LANG="en_US.UTF-8"
ARG USERNAME="vscode"
ARG USER_UID="1000"
ARG USER_GID="1000"
ARG USER_GECOS="Interactive User"
ARG COURSIER_CACHE="/root/coursier-cache"

FROM ghcr.io/rjstone/devcontainer-base:${VARIANT}-0.4.1 AS base

ENV JAVA_JDK_VERSION="17"
ENV ALMOND_VERSION="0.14.1"
ENV SCALA_VERSION="2.13.1"
ENV COURSIER_INSTALL_DIR="/usr/local/bin"
ENV UV_INSTALL_DIR="/usr/local/bin"

ENV SHELL="/bin/bash"
SHELL [ "/bin/bash", "-c" ]
USER root

FROM base AS base-jdk
WORKDIR /root
COPY install-jdk.sh /root/
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,id=apt-cache \
  --mount=type=cache,target=/var/lib/apt,sharing=locked,id=apt-lib \
  bash ./install-jdk.sh

ARG COURSIER_CACHE
FROM base-jdk AS base-scala
WORKDIR /root
COPY install-scala.sh /root/
COPY load-spinal.sc /usr/local/lib/
RUN --mount=type=cache,target=${COURSIER_CACHE},sharing=locked,id=coursier-cache \
  bash ./install-scala.sh

FROM base-scala AS base-python
WORKDIR /root
COPY install-python.sh /root/
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,id=apt-cache \
  --mount=type=cache,target=/var/lib/apt,sharing=locked,id=apt-lib \
  bash ./install-python.sh

FROM base-python AS base-jupyter
WORKDIR /root
COPY install-jupyter.sh /root/
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,id=apt-cache \
  --mount=type=cache,target=/var/lib/apt,sharing=locked,id=apt-lib \
  bash ./install-jupyter.sh

# update packages and dist-clean
FROM base-jupyter AS final
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,id=apt-cache \
  --mount=type=cache,target=/var/lib/apt,sharing=locked,id=apt-lib \
 <<EOF
apt-get update
apt-get -y autoremove
apt-get -y dist-upgrade
apt-get -y dist-clean
EOF

# Change to user home
ARG USERNAME
WORKDIR /home/${USERNAME}/
# change user, not root anymore.
ARG USERNAME
USER ${USERNAME}
COPY load-spinal.sc .

LABEL description="Development container for Scala + SpinalHDL + Jupyter" \
      author="rjstoneus@gmail.com" \
      maintainer="rjstoneus@gmail.com" \
      version="0.4.1"