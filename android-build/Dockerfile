FROM ubuntu:16.04

# System dependencies. Android SDK requires Java 8, it does not work with Java 11.
RUN apt-get update && apt-get install -y --no-install-recommends \
        sudo wget curl unzip git \
        build-essential ninja-build openjdk-8-jdk && \
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

# Golang (see https://golang.org/dl/)
RUN wget -O /go.tar.gz \
        https://dl.google.com/go/go1.13.4.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf /go.tar.gz && \
    rm /go.tar.gz
ENV PATH=$PATH:/usr/local/go/bin

# Regular user account
RUN useradd --groups sudo --create-home --shell /bin/bash user && \
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    mkdir /projects && chown user:user /projects
USER user

# Android SDK and NDK (see https://developer.android.com/studio/#downloads)
RUN wget -O ~/sdk-tools-linux.zip \
      https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip && \
    mkdir ~/android-sdk && \
    cd ~/android-sdk && \
    unzip ~/sdk-tools-linux.zip && \
    rm ~/sdk-tools-linux.zip && \
    yes | ~/android-sdk/tools/bin/sdkmanager --licenses && \
    ~/android-sdk/tools/bin/sdkmanager \
        'build-tools;28.0.3' \
        'cmake;3.6.4111459' \
        'platforms;android-28' \
        'platform-tools' \
        'ndk-bundle' \
        'emulator' \
        'system-images;android-24;default;armeabi-v7a'
ENV ANDROID_HOME=/home/user/android-sdk
ENV PATH=$PATH:$ANDROID_HOME/emulator:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools

WORKDIR /projects
