FROM ruby:2.6-alpine

LABEL build.publish.majorversion="2"
LABEL build.publish.minorversion="2.6"
LABEL build.publish.imagebase="alpine"

# Install dependencies including f360 certs and envelope
ENV ENVELOPE_VERSION 0.4.0
ENV LANG en_US.UTF-8

RUN gem install bundler -v "~> 2"

RUN apk add --no-cache \
  bash \
  curl \
  tar \
  unzip \
  ca-certificates \
  && curl -s -o /usr/local/share/ca-certificates/full360-root-ca.crt \
  https://s3.amazonaws.com/full360-cloudformation-init/public/full360-root-ca.crt \
  && update-ca-certificates \
  && curl -s -o /tmp/envelope.zip \
  https://releases-full360.s3.amazonaws.com/public/envelope/${ENVELOPE_VERSION}/envelope_${ENVELOPE_VERSION}_linux_amd64.zip \
  && unzip /tmp/envelope.zip -d /usr/local/bin/ \
  && rm /tmp/envelope.zip
