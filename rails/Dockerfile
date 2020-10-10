FROM ubuntu:focal

ENV TZ America/New_York
ENV DEBIAN_FRONTEND noninteractive
SHELL ["/bin/bash", "-c"]
LABEL maintainer="Kiren Srinivasan <srinitude@gmail.com>"

COPY . /var/app/current
WORKDIR /var/app/current

RUN chsh -s /bin/bash
RUN DEBIAN_FRONTEND=noninteractive \
    TZ=America/New_York \
    set -ex \
	\
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends \
        git \
        curl \
        emacs \
        ca-certificates \
        autoconf \
        bison \
        build-essential \
        libssl-dev \
        libyaml-dev \
        libreadline6-dev \
        zlib1g-dev \
        libncurses5-dev \
        libffi-dev \
        libgdbm6 \
        libgdbm-dev \
        libdb-dev \
        libpq-dev \
        tcl \
        tzdata \
        wget \
        ruby-full \
        rbenv \
        sudo \
	 \
    && mkdir /usr/local/share/ca-certificates/cacert.org \
    && wget -P /usr/local/share/ca-certificates/cacert.org http://www.cacert.org/certs/root.crt http://www.cacert.org/certs/class3.crt \
    && update-ca-certificates \
    && echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc \
    && echo 'eval "$(rbenv init -)"' >> ~/.bashrc \
    && source ~/.bashrc \
    && mkdir -p "$(rbenv root)"/plugins \
    && git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build \
    && rbenv install 2.7.0 \
    && rbenv rehash \
    && rbenv global 2.7.0 \
    && curl -sL https://deb.nodesource.com/setup_current.x | sudo -E bash - \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list \
    && apt -y update \
    && apt install -y --no-install-recommends \
        nodejs \
        yarn \
        postgresql \ 
        postgresql-contrib \
    \
    && ruby -v \
    && node -v \
    && yarn --version \
    && gem install bundler \
    && gem update bundler \
    && bundle install \
    && yarn install --check-files

EXPOSE 80 443 3000 5000

ENTRYPOINT [ "bundle", "exec", "rails", "s", "-p", "5000" ]
CMD [ "-b", "0.0.0.0" ]
