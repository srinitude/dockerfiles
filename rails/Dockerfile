FROM ruby:2.7.2

LABEL maintainer="Kiren Srinivasan <kiren@zestrepreneur.com>"

RUN DEBIAN_FRONTEND=noninteractive \
    TZ=America/New_York \
    set -ex \
	\
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
	&& apt update -yqq \
	&& apt install -yqq --no-install-recommends \
        apt-transport-https \
	 \
    && curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt -yqq update \
    && apt install -yqq --no-install-recommends \
        emacs \
        nodejs \
        yarn

COPY Gemfile* package.json yarn.lock /var/app/current/
WORKDIR /var/app/current/

ENV BUNDLE_PATH /gems
ENV YARN_CACHE_FOLDER /yarn

RUN mkdir -p $BUNDLE_PATH \
    && mkdir -p $YARN_CACHE_FOLDER \
    && bundle install \
    && yarn install \
    && rm -rf ./public/packs

COPY . /var/app/current/

CMD [ "./entrypoint.sh" ]
