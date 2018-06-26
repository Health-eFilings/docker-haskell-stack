FROM healthefilings/docker-ruby-node:latest
MAINTAINER Oscar Rocha <orocha@healthefilings.com>

ENV STACK_VERSION 1.6.1
ENV STACK_DOWNLOAD_URL https://github.com/commercialhaskell/stack/releases/download/v$STACK_VERSION/stack-$STACK_VERSION-linux-x86_64.tar.gz

RUN mkdir -p /root/.local/bin && \
    curl -L $STACK_DOWNLOAD_URL | tar zx -C /root/.local/bin/ --wildcards '*/stack' --strip=1  && \
    chmod +x /root/.local/bin/stack
RUN stack setup
RUN stack install hlint
RUN stack install packdeps cabal-install
RUN stack install cabal-install

RUN stack install optparse-applicative-0.13.2.0 attoparsec-0.13.2.0 \
    base-4.9.1.0 bytestring-0.10.8.1 cassava-0.4.5.1 conduit-1.2.9 \
    conduit-extra-1.1.15 containers-0.5.7.1 csv-conduit-0.6.7 \
    data-default-0.7.1.1 directory-1.3.0.0 exceptions-0.8.3 \
    optparse-generic-1.1.4 monad-logger-0.3.21 resourcet-1.1.9 text-1.2.2.1 \
    time-1.6.0.1 vector-0.12.0.1 postgresql-simple-0.5.2.1 primitive-0.6.2.0
