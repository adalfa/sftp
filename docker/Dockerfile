FROM debian:buster
MAINTAINER Adrian Dvergsdal [atmoz.net]

# Steps done in one RUN layer:
# - Install packages
# - OpenSSH needs /var/run/sshd to run
# - Remove generic host keys, entrypoint generates unique keys
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get -y -o Dpkg::Options::="--force-confdef" \
    -o Dpkg::Options::="--force-confold" install \
    acl \
    whois \
    procps \
    openssh-server \
    openssh-server \
    openssh-sftp-server \
    git-core \
    libacl1-dev\
    build-essential\
    supervisor && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /var/run/sshd && \
    rm -f /etc/ssh/ssh_host_*key* 

RUN cd /root && \
    git clone https://github.com/mysecureshell/mysecureshell && \
    cd mysecureshell && \
    ./configure --with-logcolor=yes && \
    make all && \
    make install && \
    chmod 4755 /usr/bin/mysecureshell
COPY files/sshd_config /etc/ssh/sshd_config
COPY files/create-sftp-user /usr/local/bin/
COPY files/sftp_config /etc/ssh/sftp_config
COPY files/entrypoint /

EXPOSE 22

ENTRYPOINT ["/entrypoint"]
