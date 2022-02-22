# The MIT License (MIT)
#
# Copyright (c) 2022 Jesper Pedersen <jesper.pedersen@redhat.com>
#
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the Software
# is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
# INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
# PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
#
# pgsql14-pgadmin4-rocky8
FROM centos:centos8

LABEL maintainer="Jesper Pedersen <jesper.pedersen@redhat.com>"

LABEL summary="PostgreSQL 14 pgadmin4" \
      description="PostgreSQL 14 pgadmin4"

# COPY conf/licenses /licenses

ENV PGVERSION="14"
ENV PGROOT="/usr/pgsql-${PGVERSION}"

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
 && rpm -Uvh https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm \
# && dnf -y update \
 && dnf -y module disable postgresql \
 && dnf -y install gettext hostname openssl procps-ng mod_ssl \
 && dnf -y install postgresql14 postgresql14-libs pgadmin4 pgadmin4-web \
 && dnf -y clean all

COPY root/ /

RUN mkdir -p /conf /pgconf
COPY conf/* /conf/
RUN chmod 700 /conf /pgconf

RUN cp /conf/httpd.conf /etc/httpd/conf/httpd.conf
RUN rm /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/ssl.conf

RUN mkdir -p /var/lib/pgadmin /var/log/pgadmin
COPY conf/config_local.py /var/lib/pgadmin

RUN cp /conf/pgadmin4-http.conf /var/lib/pgadmin/pgadmin4.conf
RUN ln -sf /var/lib/pgadmin/pgadmin4.conf /etc/httpd/conf.d/pgadmin4.conf

RUN cp /conf/config_local.py /var/lib/pgadmin/config_local.py
RUN ln -sf /var/lib/pgadmin/config_local.py /usr/lib/python3.6/site-packages/pgadmin4-web/config_local.py

RUN chown -R 2:0 /conf /pgconf
RUN chown -R 2:0 /usr/lib/python3.6/site-packages/pgadmin4-web \
    /var/lib/pgadmin /etc/httpd /run/httpd /var/log/httpd /var/log/pgadmin && \
    chmod -R g=u /usr/lib/python3.6/site-packages/pgadmin4-web \
    /var/lib/pgadmin /etc/httpd /run/httpd /var/log/httpd /var/log/pgadmin

EXPOSE 5050

VOLUME ["/pgconf"]

USER 2

CMD ["/usr/bin/run-pgadmin4"]
