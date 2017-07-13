FROM centos:7

# install ruby bundler dependencies
RUN yum install -y \
  make \
  gcc-c++ \
  git-core \
  libxml2-devel \
  libxslt-devel \
  mysql-devel

# install instance client
COPY config/yum.repos.d/ /etc/yum.repos.d/

RUN yum install -y \
  oracle-instantclient12.1-basic \
  oracle-instantclient12.1-devel

# oracle runtime should be set before bundle install
ENV ORACLE_HOME="/usr/lib/oracle/12.1/client64"
ENV PATH="$PATH:$ORACLE_HOME/bin"
ENV LD_LIBRARY_PATH="$ORACLE_HOME/lib"
ENV NLS_LANG="American_America.UTF8"

RUN mkdir -p /ruby-app/src \
  && mkdir /ruby-app/shared

WORKDIR /ruby-app/src

# cache bundle install
COPY Gemfile* ./

RUN bundle install -path ../shared/bundle

COPY . .

# setup utf-8 enconding for ruby files by default
ENV LC_ALL "en_US.UTF-8"

# listening port
ENV PORT 80
EXPOSE $PORT

CMD ["bundle exec rackup --host 0.0.0.0 --port $PORT"]
