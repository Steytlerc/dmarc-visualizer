FROM python:alpine 
FROM golang:alpine as build

RUN apk add build-base libxml2-dev libxslt-dev py-pip \
    && pip install parsedmarc

# install git and clone repo
RUN apk add --update curl git
RUN git clone https://github.com/maxmind/geoipupdate.git /tmp/build

# checkout tag
ARG VERSION
WORKDIR /tmp/build
RUN git checkout ${VERSION}

# build binary for alpine
ENV GOPATH /tmp/build
WORKDIR /tmp/build/cmd/geoipupdate
RUN go get -t ./... 2> /dev/null; exit 0
RUN go build

# final image
FROM alpine:latest

# install certs needed for raw binary
RUN apk add --update ca-certificates && \
    rm -rf /var/cache/apk/*

# copy the binary over from the build image, directly into bin dir
COPY --from=build /tmp/build/cmd/geoipupdate/geoipupdate /usr/bin/

# configure startup
ADD src/exec.sh /
CMD [ "/exec.sh" ]

COPY parsedmarc.ini /
COPY GeoLite2-Country.mmdb /usr/share/GeoIP/GeoLite2-Country.mmdb
