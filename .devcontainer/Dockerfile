# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.140.1/containers/cpp/.devcontainer/base.Dockerfile

# [Choice] Debian / Ubuntu version: debian-10, debian-9
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/cpp:debian

# Activity 2 (Python): pip plus the system libraries OpenCV needs.
# Activity 1 needs nothing extra - this image already ships gcc and
# the OpenSSL development headers.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends python3-pip libgl1 libglib2.0-0 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
