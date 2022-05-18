FROM python:alpine
FROM golang:alpine as build

RUN apk add build-base libxml2-dev libxslt-dev py-pip \
    && pip install parsedmarc

# install git and clone repo
