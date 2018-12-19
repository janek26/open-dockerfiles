FROM ubuntu:18.04

RUN apt-get -y update && apt-get -y install --no-install-recommends \
  ca-certificates \
  gnupg2 \
  vsftpd \
  jq \
  supervisor \
  apt-transport-https \
  s3fs \
  && rm -rf /var/lib/apt/lists/*

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
RUN echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-4.0.list
RUN apt-get -y update && apt-get -y install --no-install-recommends \
  mongodb-org-shell \
  && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /home
RUN mkdir -p /var/run/vsftpd/empty/

ADD start.sh /usr/local/

ADD vsftpd.conf /etc/vsftpd.conf

RUN chown root:root /etc/vsftpd.conf

ADD add_users_in_container.sh /usr/local/

RUN echo "/usr/sbin/nologin" >> /etc/shells

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN groupadd ftpaccess

EXPOSE 21 30000-30100

CMD ["/usr/bin/supervisord"]
