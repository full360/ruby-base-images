FROM openjdk:7-jre

LABEL build.publish.majorversion="1.7"
LABEL build.publish.imagebase="jre"

ENV ENVELOPE_VERSION 0.4.0

RUN apt-get update -qq && \
  apt-get install -y --no-install-recommends -qq \
  libc6-dev \
  git \
  tar \
  unzip \
  ca-certificates \
  netbase \
  && curl -sL https://deb.nodesource.com/setup_6.x | bash - \
  && apt-get install -y \
  nodejs \
  && curl -sL -o /usr/local/share/ca-certificates/full360-root-ca.crt \
  https://s3.amazonaws.com/full360-cloudformation-init/public/full360-root-ca.crt \
  && update-ca-certificates \
  && curl -sL -o /tmp/envelope.zip \
  https://releases-full360.s3.amazonaws.com/public/envelope/${ENVELOPE_VERSION}/envelope_${ENVELOPE_VERSION}_linux_amd64.zip \
  && unzip /tmp/envelope.zip -d /usr/local/bin/ \
  && rm /tmp/envelope.zip \
  && rm -rf /var/lib/apt/lists/*

ENV JRUBY_VERSION 1.7.27
ENV JRUBY_SHA1 4a24fe103d3735b23cc58668dec711857125a6f3

RUN mkdir /opt/jruby \
  && curl -fSL https://s3.amazonaws.com/jruby.org/downloads/${JRUBY_VERSION}/jruby-bin-${JRUBY_VERSION}.tar.gz -o /tmp/jruby.tar.gz \
  && echo "$JRUBY_SHA1 /tmp/jruby.tar.gz" | sha1sum -c - \
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

RUN gem install bundler -v 1.16.3

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
