FROM runmymind/docker-android-sdk

MAINTAINER Gabriel Goncalves <contato@gabrieldeveloper.com.br>

RUN apt-get update \

    && apt-get install -y curl zip unzip git software-properties-common vim\
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# ——————————
# Install Node and global packages
# ——————————
ENV NODE_VERSION 6.11.1
RUN cd && \
    wget -q http://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-x64.tar.gz && \
    mv node-v${NODE_VERSION}-linux-x64 /opt/node && \
    rm node-v${NODE_VERSION}-linux-x64.tar.gz
ENV PATH ${PATH}:/opt/node/bin

##
## Install react native
##
RUN npm install -g create-react-native-app

# Clean up when done.
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    npm cache clear

# Default react-native web server port
EXPOSE 8081


ENV USERNAME dev

RUN adduser --disabled-password --gecos '' $USERNAME && \
    echo $USERNAME:$USERNAME | chpasswd && \
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    adduser $USERNAME sudo

# Add Tini
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

#USER $USERNAME

# Set workdir
WORKDIR /home/$USERNAME/app

# Tell gradle to store dependencies in a sub directory of the android project
# this persists the dependencies between builds
ENV GRADLE_USER_HOME /home/$USERNAME/app/android/gradle_deps

#ENTRYPOINT ["/tini", "--"]

