FROM jenkins-with-docker:latest

USER root

RUN mkdir -p /usr/local/lib/docker/cli-plugins

COPY docker-compose /usr/local/lib/docker/cli-plugins/docker-compose

RUN chmod +x /usr/local/lib/docker/cli-plugins/docker-compose && \
    if ! getent group 983 >/dev/null; then \
        groupadd --gid 983 dockerhost; \
    fi && \
    DOCKER_GROUP=$(getent group 983 | cut -d: -f1) && \
    usermod -aG "$DOCKER_GROUP" jenkins

USER jenkins
