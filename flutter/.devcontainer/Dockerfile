FROM ubuntu:24.04

# Define Flutter version as an environment variable, with a default value
ARG SYSTEM_OS=linux
ARG FLUTTER_CHANNEL=stable
ARG FLUTTER_VERSION=

# Installing the required packages
RUN apt-get update && apt-get install -y software-properties-common curl
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update && apt-get install -y openjdk-17-jdk wget unzip git

# Change default password
RUN echo "root:root" | chpasswd
RUN echo "ubuntu:ubuntu" | chpasswd

# Show welcome message
COPY <<EOF /etc/profile.d/welcome.sh 
#!/bin/bash
echo "💪 Welcome to the Flutter Dev Container!"
echo "🤓 Some important information:"
echo ""
echo "🤖 Tool versions:"
echo "- Ubuntu version: 24.04"
echo "- Flutter version: \$(/home/ubuntu/flutter/bin/flutter --version | head -n 1)"
echo "- Android SDK version: 34.0.0-rc3"
echo ""
echo "📝 Also:"
echo "- You may need to run 'flutter doctor --android-licenses' to accept licenses the first time."
echo "- The default username is 'ubuntu' and the default password is 'ubuntu'."
echo "- For root access, the password is 'root'."
echo ""
echo "🎉 Enjoy!"
EOF

RUN chmod a+x /etc/profile.d/welcome.sh
RUN echo "source /etc/profile.d/welcome.sh" >> /home/ubuntu/.bashrc

USER ubuntu
WORKDIR /home/ubuntu

# Download and install the specified Flutter version
RUN if [ -n "$FLUTTER_VERSION" ]; then \
        echo "Let's download the specified Flutter version: $FLUTTER_VERSION"; \
        wget https://storage.googleapis.com/flutter_infra_release/releases/${FLUTTER_CHANNEL}/${SYSTEM_OS}/flutter_${SYSTEM_OS}_${FLUTTER_VERSION}-stable.tar.xz ; \
        tar xvf flutter_${SYSTEM_OS}_${FLUTTER_VERSION}-stable.tar.xz ; \
        rm flutter_${SYSTEM_OS}_${FLUTTER_VERSION}-stable.tar.xz ; \
    else \
        echo "Let's download the latest Flutter version"; \
        git clone -b stable https://github.com/flutter/flutter.git ; \
        ./flutter/bin/flutter --version ; \
    fi

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

ENTRYPOINT ["/bin/bash", "-l", "-c"]
