FROM ubuntu:24.04

# Define Flutter version as an environment variable, with a default value
ARG FLUTTER_VERSION=3.13.3

# Installing the required packages
RUN apt-get update && apt-get install -y software-properties-common
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
echo "- Flutter version: ${FLUTTER_VERSION}"
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

ENTRYPOINT ["/bin/bash", "-l", "-c"]