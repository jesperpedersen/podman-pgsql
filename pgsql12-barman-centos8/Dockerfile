# The MIT License (MIT)
#
# Copyright (c) 2020 Jesper Pedersen <jesper.pedersen@redhat.com>
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
# pgsql12-barman-centos8
FROM centos:centos8

LABEL maintainer="Jesper Pedersen <jesper.pedersen@redhat.com>"

LABEL summary="PostgreSQL 12 Barman" \
      description="PostgreSQL 12 Barman"

# COPY conf/licenses /licenses

ENV PGVERSION="12"
ENV PGROOT="/usr/pgsql-${PGVERSION}"

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
 && rpm -Uvh https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm \
 && dnf -y update \
 && dnf -y module disable postgresql \
 && dnf -y install wget tar \
 && dnf -y install postgresql12 postgresql12-libs barman crontabs \
 && dnf -y clean all

COPY root/ /

RUN mkdir -p /conf /pgconf /pgbackup
COPY conf/* /conf/
RUN chown barman:barman /etc/barman.conf
RUN chown -R barman:barman /conf /pgconf /pgbackup
RUN chmod 700 /conf /pgconf /pgbackup

RUN mkdir -p /usr/local/bin
RUN cd /tmp \
 && wget https://github.com/prometheus/node_exporter/releases/download/v1.0.0-rc.0/node_exporter-1.0.0-rc.0.linux-amd64.tar.gz \
 && tar xzf node_exporter-1.0.0-rc.0.linux-amd64.tar.gz \
 && mv node_exporter-1.0.0-rc.0.linux-amd64/node_exporter /usr/local/bin/node_exporter \
 && rm -Rf node_exporter-1.0.0-rc.0.linux-amd64 node_exporter-1.0.0-rc.0.linux-amd64.tar.gz \
 && chown barman:barman /usr/local/bin/node_exporter

VOLUME ["/pgconf", "/pgbackup"]

USER barman

CMD ["/usr/bin/run-barman"]
