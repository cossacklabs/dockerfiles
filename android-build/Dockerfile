FROM ubuntu:22.04

# System dependencies. Android SDK requires Java 8, it does not work with Java 11.
# Gradle requires Java 11, it does not work with Java 8.
RUN apt-get update && apt-get install -y --no-install-recommends \
        sudo wget curl unzip git gpg coreutils \
        build-essential ninja-build \
        openjdk-8-jre openjdk-11-jdk && \
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
      https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip && \
    echo "2ccbda4302db862a28ada25aa7425d99dce9462046003c1714b059b5c47970d8  /home/user/cmdline-tools-linux.zip" | sha256sum --check - && \
    mkdir ~/android-sdk && \
    cd ~/android-sdk && \
    unzip ~/cmdline-tools-linux.zip && \
    rm ~/cmdline-tools-linux.zip && \
    mv cmdline-tools latest && mkdir cmdline-tools && mv latest cmdline-tools && \
    yes | ~/android-sdk/cmdline-tools/latest/bin/sdkmanager --licenses && \
    ~/android-sdk/cmdline-tools/latest/bin/sdkmanager \
        'build-tools;28.0.3' \
        'cmake;3.6.4111459' \
        'platforms;android-28' \
        'platform-tools' \
        'ndk-bundle' \
        'emulator' \
        'system-images;android-24;default;armeabi-v7a'
ENV ANDROID_HOME=/home/user/android-sdk
ENV PATH=$PATH:$ANDROID_HOME/emulator:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools

WORKDIR /projects
