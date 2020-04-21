FROM healthefilings/docker-ruby-node:2.3.7

ENV GO_VERSION_FILE go1.14.2.linux-amd64.tar.gz
RUN wget https://dl.google.com/go/$GO_VERSION_FILE
RUN tar -C /usr/local -xzf $GO_VERSION_FILE
ENV GOPATH /root/go
ENV PATH $PATH:/usr/local/go/bin:$GOPATH/bin

RUN go get -u golang.org/x/tools/cmd/godoc
RUN go get -u github.com/onsi/ginkgo/ginkgo
RUN go get -u github.com/onsi/gomega/...
RUN go get -u github.com/gobuffalo/packr/packr
