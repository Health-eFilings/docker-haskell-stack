FROM healthefilings/docker-ruby-node:2.4.4
MAINTAINER Oscar Rocha <orocha@healthefilings.com>

ENV STACK_VERSION 1.6.1
ENV STACK_DOWNLOAD_URL https://github.com/commercialhaskell/stack/releases/download/v$STACK_VERSION/stack-$STACK_VERSION-linux-x86_64.tar.gz

RUN mkdir -p /root/.local/bin && \
    curl -L $STACK_DOWNLOAD_URL | tar zx -C /root/.local/bin/ --wildcards '*/stack' --strip=1  && \
    chmod +x /root/.local/bin/stack

RUN stack setup --resolver lts-8.6
RUN stack install hlint
RUN stack install packdeps
RUN stack install cabal-install

RUN stack --resolver lts-8.6 build optparse-applicative \
    attoparsec base bytestring cassava \
    conduit conduit-extra containers csv-conduit \
    data-default directory exceptions \
    optparse-generic monad-logger resourcet text \
    time vector postgresql-simple primitive \
    xml scientific aeson raw-strings-qq split process process-extras

RUN apt-get -y --force-yes install nano
