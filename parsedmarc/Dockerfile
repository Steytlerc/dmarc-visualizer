FROM python:alpine

RUN apk update
RUN apk add build-base libxml2-dev libxslt-dev python3-dev \ 
    && pip install parsedmarc

COPY parsedmarc.ini /
COPY GeoLite2-Country.mmdb /usr/share/GeoIP/GeoLite2-Country.mmdb

