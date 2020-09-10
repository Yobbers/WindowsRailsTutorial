FROM ruby

WORKDIR /home/user/workdirectory

ENV PORT 3000

EXPOSE $PORT

RUN gem install rails bundler
RUN gem install rails

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

#Install packages
RUN apt-get update -qq && apt-get install -y build-essential nodejs postgresql-client yarn \
 && rm -rf /var/lib/apt/lists/* \
 && curl -o- -L https://yarnpkg.com/install.sh | bash

ENTRYPOINT [ "/bin/bash" ]
