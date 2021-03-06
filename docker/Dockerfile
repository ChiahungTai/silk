FROM ubuntu:14.04

MAINTAINER mvine@silklabs.com

RUN apt-get update -qq
RUN apt-get install -y software-properties-common python-software-properties
RUN add-apt-repository -y ppa:george-edison55/cmake-3.x
RUN add-apt-repository -y ppa:git-core/ppa
RUN add-apt-repository -y ppa:kubuntu-ppa/backports
RUN add-apt-repository -y ppa:phablet-team/tools
RUN add-apt-repository -y ppa:ubuntu-toolchain-r/ppa
RUN add-apt-repository -y ppa:amarburg/opencv3
RUN apt-get update

ENV JAVA_HOME /usr
RUN echo 'PATH=$PATH:HOME/bin:$JAVA_HOME/bin' >> /etc/profile &&\
    echo 'export JAVA_HOME' >> /etc/profile &&\
    echo 'export PATH' >> /etc/profile

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
  android-tools-adb \
  android-tools-fastboot \
  autoconf \
  apt-file \
  bc \
  ccache \
  chrpath \
  cmake \
  diffstat \
  g++ \
  g++-4.8 \
  g++-4.8-multilib \
  gawk \
  git \
  lib32stdc++6 \
  lib32z1 \
  libc6-dev \
  libc6-dev-i386 \
  libtool \
  libopencv3-dev \
  libxml2-utils \
  linux-libc-dev  \
  m4 \
  mkisofs \
  openjdk-7-jdk \
  openssh-server \
  texinfo \
  zip

USER root
# SSH login fix so user isn't kicked after login
RUN sed 's#session\s*required\s*pam_loginuid.so#session optional pam_loginuid.so#g' -i /etc/pam.d/sshd
COPY ./51-android.rules /etc/udev/rules.d/51-android.rules
RUN udevadm trigger

RUN rm -rf /etc/ssh/ssh_host_dsa_key /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key

RUN addgroup silk
RUN adduser -ingroup silk --gecos "" --disabled-password silk
COPY sudoers_silk /etc/sudoers.d/silk

# Install node v6.9.1
RUN wget https://nodejs.org/dist/v6.9.1/node-v6.9.1-linux-x64.tar.gz
RUN tar -xv -C /usr/local --strip-components 1 -f node-v6.9.1-linux-x64.tar.gz

USER silk
RUN ccache -M 10GB

RUN ssh-keygen -q -N "" -t rsa -f /home/silk/.ssh/id_rsa
RUN cp /home/silk/.ssh/id_rsa.pub /home/silk/.ssh/authorized_keys
# add localhost to silk's list of known_hosts files without need ssh login
RUN ssh-keyscan -H localhost >> ~/.ssh/known_hosts

# add github to the list of know hosts
RUN ssh-keyscan -H github.com >> ~/.ssh/known_hosts

# Setup git
RUN git config --global user.email "silkysmooth@example.com"
RUN git config --global user.name "Silky Smooth"
RUN git config --global color.ui true

