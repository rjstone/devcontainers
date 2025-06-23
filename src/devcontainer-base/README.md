# devcontainer-base

Pre-build:
```
ghcr.io/rjstone/devcontainer-base
```

This is a more stripped down version of the official devcontainer base images,
focusing on Ubuntu. It attempts to inherit the user's `LANG` environment
variable, creates a `vscode` non-root user, and installs only the most basic
things needed for interactive use and software installation.

Packages: lsb-release init-system-helpers sudo man-db manpages locales bash-completion curl apt-utils unzip zip gzip bzip2 xz-utils openssh-client gpg ca-certificates

Languages: In addition to whatever the user selects or is inherited from
their environment, English and `C.UTF-8` will also be installed.

And that's it. Not as small as Alpine Linux, but not too bad for Ubuntu.