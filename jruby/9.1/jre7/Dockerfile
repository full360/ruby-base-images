FROM openjdk:7-jre

LABEL build.publish.minorversion="9.1"
LABEL build.publish.imagebase="jre7"

RUN apt-get update && apt-get install -y libc6-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*

ENV JRUBY_VERSION 9.1.17.0
ENV JRUBY_SHA256 6a22f7bf8fef1a52530a9c9781a9d374ad07bbbef0d3d8e2af0ff5cbead0dfd5
RUN mkdir /opt/jruby \
  && curl -fSL https://repo1.maven.org/maven2/org/jruby/jruby-dist/${JRUBY_VERSION}/jruby-dist-${JRUBY_VERSION}-bin.tar.gz -o /tmp/jruby.tar.gz \
  && echo "$JRUBY_SHA256 /tmp/jruby.tar.gz" | sha256sum -c - \
  && tar -zx --strip-components=1 -f /tmp/jruby.tar.gz -C /opt/jruby \
  && rm /tmp/jruby.tar.gz \
  && update-alternatives --install /usr/local/bin/ruby ruby /opt/jruby/bin/jruby 1
ENV PATH /opt/jruby/bin:$PATH

# skip installing gem documentation
RUN mkdir -p /opt/jruby/etc \
	&& { \
		echo 'install: --no-document'; \
		echo 'update: --no-document'; \
	} >> /opt/jruby/etc/gemrc

RUN gem install bundler

# install things globally, for great justice
# and don't create ".bundle" in all our apps
ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
	BUNDLE_BIN="$GEM_HOME/bin" \
	BUNDLE_SILENCE_ROOT_WARNING=1 \
	BUNDLE_APP_CONFIG="$GEM_HOME"
ENV PATH $BUNDLE_BIN:$PATH
RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
	&& chmod 777 "$GEM_HOME" "$BUNDLE_BIN"

CMD [ "irb" ]

ENV ENVELOPE_VERSION 0.4.0

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
