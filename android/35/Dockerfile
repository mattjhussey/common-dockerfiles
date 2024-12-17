# Set variables
ARG ANDROID_COMPILE_SDK="35"
ARG ANDROID_BUILD_TOOLS="35.0.0"
ARG ANDROID_SDK_TOOLS="11076708"
FROM eclipse-temurin:17-jdk-noble AS base
    ARG ANDROID_COMPILE_SDK
    ARG ANDROID_BUILD_TOOLS
    ARG ANDROID_SDK_TOOLS

    # Install Dependencies
    RUN apt-get --quiet update --yes
    RUN apt-get --quiet install --yes wget unzip

    ENV ANDROID_HOME="/opt/android-sdk"
    # Make the android sdk directory
    RUN mkdir --parents ${ANDROID_HOME}

    # Here we are installing androidSDK tools from official source,
    # (the key thing here is the url from where you are downloading these sdk tool for command line, so please do note this url pattern there and here as well)
    # after that unzipping those tools and
    # then running a series of SDK manager commands to install necessary android SDK packages that'll allow the app to build
    RUN wget --no-verbose --output-document=${ANDROID_HOME}/cmdline-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS}_latest.zip
    RUN unzip -q -d "$ANDROID_HOME/cmdline-tools" "$ANDROID_HOME/cmdline-tools.zip"
    RUN mv -T "$ANDROID_HOME/cmdline-tools/cmdline-tools" "$ANDROID_HOME/cmdline-tools/tools"

    ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/cmdline-tools/tools/bin

    # Nothing fancy here, just checking sdkManager version
    RUN sdkmanager --version

    # use yes to accept all licenses
    RUN yes | sdkmanager --licenses > /dev/null || true
    RUN sdkmanager "platforms;android-${ANDROID_COMPILE_SDK}"
    RUN sdkmanager "platform-tools"
    RUN sdkmanager "build-tools;${ANDROID_BUILD_TOOLS}"

# Cleaning
FROM scratch AS cleanup
    COPY --from=base / /
    ENV ANDROID_HOME="/opt/android-sdk"
    ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/cmdline-tools/tools/bin
