## Image name: faucet/gnmi

FROM ubuntu:16.04

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -qy --no-install-recommends \
      apt-utils \
      ca-certificates \
      software-properties-common \
    && add-apt-repository -y ppa:longsleep/golang-backports \
    && apt-get update

RUN DEBIAN_FRONTEND=noninteractive apt-get install -qy --no-install-recommends \
    fping \
    git \
    golang-go \
    iproute2 \
    iputils-ping \
    net-tools \
    netcat-openbsd \
    openssh-client \
    psmisc \
    sudo \
    vim

ENV HOME=/home/faucet
RUN mkdir $HOME
WORKDIR $HOME

COPY ./ .

ENV GOPATH=$HOME/go
ENV GOBIN=$GOPATH/bin
ENV PATH=$GOBIN:${PATH}

RUN mkdir -p \
      $GOPATH \
    && go get -u \
      github.com/google/gnxi/gnmi_capabilities \
      github.com/google/gnxi/gnmi_get \
      github.com/google/gnxi/gnmi_set \
      github.com/google/gnxi/gnmi_target

RUN go install -v \
      github.com/google/gnxi/gnmi_capabilities \
      github.com/google/gnxi/gnmi_get \
      github.com/google/gnxi/gnmi_set \
      github.com/google/gnxi/gnmi_target


RUN cd $HOME/certs/ \
    && ./generate.sh

ENV GNMI_TARGET=localhost
ENV GNMI_PORT=10161

CMD ./_startup.sh \
    && /bin/bash
