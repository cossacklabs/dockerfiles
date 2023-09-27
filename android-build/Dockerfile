FROM ubuntu:23.04

# System dependencies. Android SDK requires Java 8, it does not work with Java 11.
# Gradle requires Java 11, it does not work with Java 8.
RUN apt-get update && apt-get install -y --no-install-recommends \
        sudo wget curl unzip git gpg coreutils \
        build-essential ninja-build \
        openjdk-17-jre openjdk-17-jdk && \
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

# Golang (see https://golang.org/dl/)
RUN wget -O /go.tar.gz \
        https://dl.google.com/go/go1.19.linux-amd64.tar.gz && \
    echo "464b6b66591f6cf055bc5df90a9750bf5fbc9d038722bb84a9d56a2bea974be6  /go.tar.gz" | sha256sum --check - && \
    tar -C /usr/local -xzf /go.tar.gz && \
    rm /go.tar.gz
ENV PATH=$PATH:/usr/local/go/bin

# Regular user account
RUN useradd --groups sudo --create-home --shell /bin/bash user && \
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    mkdir /projects && chown user:user /projects
USER user

# Android SDK and NDK (see https://developer.android.com/studio/#downloads)
RUN wget -O ~/cmdline-tools-linux.zip \
      https://dl.google.com/android/repository/commandlinetools-linux-10406996_latest.zip && \
    echo "8919e8752979db73d8321e9babe2caedcc393750817c1a5f56c128ec442fb540  /home/user/cmdline-tools-linux.zip" | sha256sum --check - && \
    mkdir ~/android-sdk && \
    cd ~/android-sdk && \
    unzip ~/cmdline-tools-linux.zip && \
    rm ~/cmdline-tools-linux.zip && \
    mv cmdline-tools latest && mkdir cmdline-tools && mv latest cmdline-tools && \
    yes | ~/android-sdk/cmdline-tools/latest/bin/sdkmanager --licenses && \
    ~/android-sdk/cmdline-tools/latest/bin/sdkmanager \
        'build-tools;34.0.0' \
        'cmake;3.22.1' \
        'platforms;android-31' \
        'ndk;26.0.10792818'
ENV ANDROID_HOME=/home/user/android-sdk
ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin

WORKDIR /projects
