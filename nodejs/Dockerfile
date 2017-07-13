FROM centos:7

LABEL description="Test web server"

ARG SSH_KEY

# using s6 as pid 1 supervisor ################################################
ADD https://github.com/just-containers/s6-overlay/releases/download/v1.19.1.1/s6-overlay-amd64.tar.gz /tmp
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C / --exclude="./bin" \
  && tar xzf /tmp/s6-overlay-amd64.tar.gz -C /usr ./bin

ENTRYPOINT ["/init"]
###############################################################################

# install nodejs
RUN curl --silent --location https://rpm.nodesource.com/setup_6.x | bash - \
  && yum -y install nodejs

# install dependencies
RUN yum -y install \
  git-core \
  bzip2 \
  fontconfig \
  freetype \
  open-sans-fonts

# install app
RUN mkdir /app

WORKDIR /app

COPY package.json /app

RUN ssh-keyscan -H github.com > /etc/ssh/ssh_known_hosts
  && eval "$(ssh-agent -s)" > /dev/null \
  && echo "$SSH_KEY" | ssh-add - \
  && git clone -b docker --single-branch git@github.com:app/app.git . \
  && npm install --only=production \
  && npm cache clean

COPY . /app

ENV PORT 3000
EXPOSE $PORT

# setup s6
COPY services.d/ /etc/services.d/
