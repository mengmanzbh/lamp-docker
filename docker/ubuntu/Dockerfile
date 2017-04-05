FROM ubuntu:16.04

MAINTAINER toi.xtran <toi.xtran@gmail.com>

# Env vars define
ENV ROOT_PASSWD="toor"

# Update & upgrade
RUN apt-get update && \
    apt-get -y upgrade

# Install common packages
RUN apt-get -y install \
    python \
    vim \
    wget \
    curl  \
    git-all \
    openssh-server \
    supervisor

# Config ssh
RUN mkdir /var/run/sshd && \
    echo "root:${ROOT_PASSWD}" | chpasswd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Config supervisord
ADD /root/etc/supervisord.conf /etc/supervisord.conf
ADD /root/etc/supervisord.d /etc/supervisord.d

# Clean
RUN rm -rf /var/lib/apt/lists/*

EXPOSE 22

CMD ["/usr/bin/supervisord", "--configuration=/etc/supervisord.conf"]
