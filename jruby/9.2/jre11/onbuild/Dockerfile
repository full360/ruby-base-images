FROM full360/jruby:9.2-jre11

LABEL build.publish.majorversion="9.2"
LABEL build.publish.imagebase="jre11-onbuild"

ENV RACK_ENV production

# throw errors if Gemfile has been modified since Gemfile.lock
RUN bundle config --global frozen 1
RUN bundle config set without "test development"

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

ONBUILD COPY Gemfile      /usr/src/app/
ONBUILD COPY Gemfile.lock /usr/src/app/
ONBUILD RUN bundle install

ONBUILD COPY . /usr/src/app
ONBUILD RUN rm -f .env
