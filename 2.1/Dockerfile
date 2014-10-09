FROM betacz/baseimage-zh
MAINTAINER Beta CZ <hlj8080@gmail.com>

ENV HOME /root

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# ruby 2.1.3
# Based: https://github.com/docker-library/ruby/blob/master/2.1/Dockerfile
RUN apt-get update && apt-get install -y curl \
      procps \
      build-essential \
      zlib1g-dev \
      libssl-dev \
      libreadline6-dev \
      libyaml-dev

ENV RUBY_MAJOR 2.1
ENV RUBY_VERSION 2.1.3

# some of ruby's build scripts are written in ruby
# we purge this later to make sure our final image uses what we just built
RUN  apt-get install -y bison ruby \
  && mkdir -p /usr/src/ruby \
  && curl -SL "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.bz2" \
    | tar -xjC /usr/src/ruby --strip-components=1 \
  && cd /usr/src/ruby \
  && autoconf \
  && ./configure --disable-install-doc \
  && make -j"$(nproc)" \
  && apt-get purge -y --auto-remove bison ruby \
  && make install \
  && rm -r /usr/src/ruby

# Set the gem sources to ruby.taobao.org
RUN gem sources --remove https://rubygems.org/
RUN gem sources -a http://mirrors.aliyun.com/rubygems/

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> .gemrc

RUN gem install bundler

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
