FROM jruby:9.1-jre-alpine

LABEL build.publish.minorversion="9.1"
LABEL build.publish.imagebase="jre8-alpine"

ENV ENVELOPE_VERSION 0.3.3

# Install dependencies including f360 certs and envelope
RUN apk add --no-cache \
     bash \
     curl \
     tar \
     unzip \
     ca-certificates \
     nodejs-lts \
     && curl -s -o /usr/local/share/ca-certificates/full360-root-ca.crt \
     https://s3.amazonaws.com/full360-cloudformation-init/public/full360-root-ca.crt \
     && update-ca-certificates \
     && curl -s -o /tmp/envelope.zip \
     https://releases-full360.s3.amazonaws.com/public/envelope/${ENVELOPE_VERSION}/envelope_${ENVELOPE_VERSION}_linux_amd64.zip \
     && unzip /tmp/envelope.zip -d /usr/local/bin/ \
     && rm /tmp/envelope.zip
