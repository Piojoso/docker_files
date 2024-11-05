FROM ubuntu:24.04

# Define Flutter version as an environment variable, with a default value
ARG FLUTTER_VERSION=3.13.3

# Installing the required packages
RUN apt-get update && apt-get install -y software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update && apt-get install -y openjdk-17-jdk wget unzip git

# Change default password
RUN echo "root:root" | chpasswd

USER ubuntu
WORKDIR /home/ubuntu

# Download and install the specified Flutter version
RUN wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_${FLUTTER_VERSION}-stable.tar.xz
RUN tar xvf flutter_linux_${FLUTTER_VERSION}-stable.tar.xz
RUN rm flutter_linux_${FLUTTER_VERSION}-stable.tar.xz

# Installing Android SDK
RUN wget https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip
RUN mkdir android-sdk && unzip -d /tmp commandlinetools-linux-8512546_latest.zip
RUN rm commandlinetools-linux-8512546_latest.zip
RUN mkdir -p android-sdk/cmdline-tools/latest
RUN mv /tmp/cmdline-tools/* android-sdk/cmdline-tools/latest/
ENV ANDROID_HOME="/home/ubuntu/android-sdk"
ENV PATH="$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator:/home/ubuntu/flutter/bin:$PATH"
RUN echo "y" | sdkmanager --install "build-tools;34.0.0-rc3"
RUN echo "y" | sdkmanager --install "platform-tools"
RUN sdkmanager --install "platforms;android-34"
RUN sdkmanager --install "emulator"
