FROM ubuntu:18.04

RUN apt-get update && apt-get install -y --no-install-recommends \
      sudo less nano vim \
      git curl wget \
      tar unzip \
      build-essential \
      clang gcc \
      clang-format \
      gdb lldb elfutils strace \
      libssl-dev \
 && apt-get -y autoremove && apt-get -y clean \
 && rm -rf /var/lib/apt/lists/*

RUN useradd --groups sudo --create-home --shell /bin/bash user \
 && echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers \
 && mkdir /projects && chown user:user /projects
USER user
WORKDIR /projects
