FROM php:8.0.2-fpm

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Andre Goncalves <andreseko@gmail.com>" \
  PHP="8.0" \
  NODE="14" \
  org.label-schema.name="andreseko/gitlab-ci-pipeline-php" \
  org.label-schema.description=":coffee: Docker images for build and test PHP applications with Gitlab CI (or any other CI plataform!)" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.schema-version="1.0" \
  org.label-schema.vcs-url="https://github.com/andreseko/gitlab-ci-pipeline-php" \
  org.label-schema.vcs-ref=$VCS_REF

# Set correct environment variables
ENV IMAGE_USER=php
ENV HOME=/home/$IMAGE_USER
ENV COMPOSER_HOME=$HOME/.composer
ENV PATH=$HOME/.yarn/bin:$PATH
ENV GOSS_VERSION="0.3.16"
ENV PHP_VERSION=8.0

USER root

WORKDIR /tmp

# COPY INSTALL SCRIPTS
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
COPY ./php/scripts/*.sh /tmp/
RUN chmod +x /tmp/*.sh
RUN mkdir -p /opt/oracle

# Install
RUN bash ./packages.sh \
  && bash ./extensions.sh \
  && bash ./node.sh \
  && adduser --disabled-password --gecos "" $IMAGE_USER && \
  echo "$IMAGE_USER  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers && \
  mkdir -p /var/www/html && \
  chown -R $IMAGE_USER:$IMAGE_USER /var/www $HOME \
  && sudo mkdir $COMPOSER_HOME && sudo rm -rf $COMPOSER_HOME/cache/* \
  && sudo chown -R $IMAGE_USER:$IMAGE_USER $COMPOSER_HOME \
  && bash ./cleanup.sh

USER $IMAGE_USER

WORKDIR /var/www/html
