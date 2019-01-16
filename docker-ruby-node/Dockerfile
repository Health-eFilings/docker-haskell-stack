FROM buildpack-deps:trusty
MAINTAINER Oscar Rocha <orocha@healthefilings.com>

# ENV configurations
ENV DEBIAN_FRONTEND noninteractive
ENV PATH $PATH:/root/.local/bin
ENV LANG C.UTF-8

# Required packages
RUN apt-get update -q && \
    apt-get install -qy libgmp-dev git-core curl zlib1g-dev build-essential \
    libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt1-dev \
    libcurl4-openssl-dev python-software-properties libffi-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install PG Client, this code installs pg client in the same way that
# https://github.com/musicglue/heroku-buildpack-postgresql
ENV POSTGRESQL_VERSION 9.5.2
ENV S3_BUCKET musicglue-buildpacks
ENV PG_LOCATION /vendor/postgresql
ENV PACKAGE https://$S3_BUCKET.s3.amazonaws.com/postgres/cedar-14/$POSTGRESQL_VERSION.tgz
ENV PATH $PATH:$PG_LOCATION/bin
ENV DOCKERIZE_VERSION v0.3.0

RUN mkdir -p $PG_LOCATION
RUN curl $PACKAGE -s -o - | tar xzf - -C $PG_LOCATION

# Install Ruby
ENV RUBY_BASE_VERSION 2.4
ENV RUBY_VERSION 2.4.4
ENV PHANTOMJS_VERSION 1.9.8

RUN echo "deb http://ftp.debian.org/debian wheezy main" >> /etc/apt/sources.list
RUN apt-get update

RUN curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
RUN apt-get install -y nodejs

RUN cd
RUN wget http://ftp.ruby-lang.org/pub/ruby/$RUBY_BASE_VERSION/ruby-$RUBY_VERSION.tar.gz
RUN tar -xzvf ruby-$RUBY_VERSION.tar.gz
RUN cd ruby-$RUBY_VERSION/ && ./configure && make && sudo make install
RUN gem install bundler
RUN npm -g install bower
RUN npm -g install phantomjs@$PHANTOMJS_VERSION

# Install Dockerize, this is a useful tool in circle ci in order to verify
# that all services are running
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && sudo tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

# This is a hack to update hosts file
RUN mkdir -p /override_lib && cp /etc/hosts /var/ && cp /lib/x86_64-linux-gnu/libnss_files.so.2 /override_lib
RUN sed -ie 's:/etc/hosts:/var/hosts:g' /override_lib/libnss_files.so.2
ENV LD_LIBRARY_PATH /override_lib
RUN rm /var/hosts
RUN echo "127.0.0.1    localhost" >> /var/hosts
