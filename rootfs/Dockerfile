FROM alpine:3.2

# install common packages
RUN apk add --update-cache curl bash sudo && rm -rf /var/cache/apk/*

# install etcdctl
RUN curl -sSL -o /usr/local/bin/etcdctl https://s3-us-west-2.amazonaws.com/get-deis/etcdctl-v0.4.9 \
    && chmod +x /usr/local/bin/etcdctl

# install confd
RUN curl -sSL -o /usr/local/bin/confd https://github.com/kelseyhightower/confd/releases/download/v0.10.0/confd-0.10.0-linux-amd64 \
    && chmod +x /usr/local/bin/confd

# define execution environment
CMD ["/app/bin/boot"]
EXPOSE 8000

# define work environment
WORKDIR /app

ADD build.sh /app/tmp/build.sh

ADD requirements.txt /app/requirements.txt

RUN DOCKER_BUILD=true /app/tmp/build.sh && rm -rf /app/*

ADD . /app

RUN ./manage.py collectstatic --settings=deis.settings --noinput  # create static resources

ENV DEIS_RELEASE 1.13.0-dev