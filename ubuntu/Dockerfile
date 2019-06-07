FROM adoptopenjdk/openjdk8:x86_64-ubuntu-jdk8u192-b12

VOLUME /data/teamcity_agent/conf

ENV CONFIG_FILE=/data/teamcity_agent/conf/buildAgent.properties \
    LANG=C.UTF-8

LABEL dockerImage.teamcity.version="latest" \
      dockerImage.teamcity.buildNumber="latest"

COPY run-agent.sh /run-agent.sh
COPY run-services.sh /run-services.sh
COPY dist/buildagent /opt/buildagent

RUN apt-get update && \
    apt-get install -y git mercurial apt-transport-https ca-certificates software-properties-common && \
    \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
    \
    apt-cache policy docker-ce && \
    apt-get update && \
    apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu systemd && \
    apt-get install -y nodejs && \
    apt-get install -y npm && \
    systemctl disable docker && \
    curl -SL https://github.com/docker/compose/releases/download/1.23.2/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose && \
    \
    apt-get install -y --no-install-recommends \
            libc6 \
            libgcc1 \
            libgssapi-krb5-2 \
            libicu60 \
            liblttng-ust0 \
            libssl1.0.0 \
            libstdc++6 \
            zlib1g \
        && rm -rf /var/lib/apt/lists/* && \
    \
    apt-get clean all && \
    \
    usermod -aG docker buildagent

# A better fix for TW-52939 Dockerfile build fails because of aufs
VOLUME /var/lib/docker

CMD ["/run-services.sh"]

EXPOSE 9090
