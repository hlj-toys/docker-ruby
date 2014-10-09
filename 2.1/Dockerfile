FROM betacz/baseimage-zh
MAINTAINER Beta CZ <hlj8080@gmail.com>

ENV HOME /root

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# ruby 2.1.3
# From: https://github.com/docker-library/ruby/blob/master/2.1/Dockerfile
RUN apt-get update && apt-get install -y curl \
      procps\
      build-essential \
      zlib1g-dev \
      libssl-dev \
      libreadline6-dev \
      libyaml-dev

ENV RUBY_MAJOR 2.1
ENV RUBY_VERSION 2.1.3

ADD http://ruby.taobao.org/mirrors/ruby/ruby-$RUBY_VERSION.tar.gz /tmp/
RUN cd /tmp && \
      tar -xzf ruby-$RUBY_VERSION.tar.gz && \
      cd ruby-$RUBY_VERSION && \
      ./configure && \
      make && \
      make install && \
      cd .. && \
      rm -rf ruby-$RUBY_VERSION && \
      rm -f ruby-$RUBY_VERSION.tar.gz

# skip installing gem documentation
RUN echo 'gem: --no-rdoc --no-ri' >> .gemrc

RUN gem install bundler

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
