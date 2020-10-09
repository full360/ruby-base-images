FROM jruby:9.2-jre8

LABEL build.publish.majorversion="9.2"
LABEL build.publish.imagebase="jre8"

ENV ENVELOPE_VERSION 0.3.3

# Install dependencies including f360 certs and envelope
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  unzip \
  ca-certificates \
  && rm -rf /var/lib/apt/lists/* \
  && curl -s -o /usr/local/share/ca-certificates/full360-root-ca.crt \
  https://s3.amazonaws.com/full360-cloudformation-init/public/full360-root-ca.crt \
  && update-ca-certificates \
  && curl -s -o /tmp/envelope.zip \
  https://releases-full360.s3.amazonaws.com/public/envelope/${ENVELOPE_VERSION}/envelope_${ENVELOPE_VERSION}_linux_amd64.zip \
  && unzip /tmp/envelope.zip -d /usr/local/bin/ \
  && rm /tmp/envelope.zip
