# syntax=docker/dockerfile:1
# for format details, see https://docs.docker.com/engine/reference/builder/

ARG VARIANT="noble"

ARG LANG="en_US.UTF-8"

ARG USERNAME="vscode"
ARG USER_UID="1000"
ARG USER_GID="1000"
ARG USER_GECOS="Interactive User"

# Official Docker "BuildPack" image (Ubuntu)
FROM buildpack-deps:${VARIANT}-curl AS base

ENV SHELL="/bin/bash"
SHELL [ "/bin/bash", "-c" ]

ARG VARIANT
RUN if [ "$VARIANT" = "noble" ]; then \
        if id "ubuntu" &>/dev/null; then \
            echo "Deleting user 'ubuntu'  for $VARIANT" && userdel -f -r ubuntu || echo "Failed to delete ubuntu user for VARIANT";\
        else \
            echo "User 'ubuntu' does not exist for $VARIANT"; \
        fi; \
    fi

# Performm basic OS configuration for interactive use
ARG LANG
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,id=apt-cache \
  --mount=type=cache,target=/var/lib/apt,sharing=locked,id=apt-lib \
  <<EOF
export DEBIAN_FRONTEND=noninteractive
apt-get update
apt-get install -y --no-install-recommends lsb-release init-system-helpers \
  sudo man-db manpages locales bash-completion curl apt-utils \
  unzip zip gzip bzip2 xz-utils openssh-client gpg ca-certificates
locale-gen "$LANG" "en_US.UTF-8" "C.UTF-8"
apt-get -y autoremove
apt-get -y dist-upgrade
apt-get -y dist-clean
EOF

# add the default non-root user with sudo permission
ARG USERNAME
ARG USER_UID
ARG USER_GID
ARG USER_GECOS
RUN <<EOF
groupadd --gid $USER_GID $USERNAME
useradd -s /bin/bash --uid $USER_UID --gid $USER_GID --comment "$USER_GECOS" -m $USERNAME
echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME
chmod 0440 /etc/sudoers.d/$USERNAME
EOF

# Change to user home
ARG USERNAME
WORKDIR /home/${USERNAME}/
# change user, not root anymore.
ARG USERNAME
USER ${USERNAME}

LABEL description="Ubuntu Development Container Base" \
      author="rjstoneus@gmail.com" \
      maintainer="rjstoneus@gmail.com" \
      version="0.4.1"

LABEL org.opencontainers.image.source="https://github.com/rjstone/devcontainers"
LABEL org.opencontainers.image.description="This is similar to the official devcontainer base images, but is a little bit more stripped down. No zsh, etc, is provided, only things needed for installation of more software and basic interactive use."