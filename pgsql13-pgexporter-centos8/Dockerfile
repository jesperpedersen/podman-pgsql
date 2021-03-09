# The MIT License (MIT)
#
# Copyright (c) 2021 Jesper Pedersen <jesper.pedersen@redhat.com>
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
# pgsql13-pgexporter-centos8
FROM centos:centos8

LABEL maintainer="Jesper Pedersen <jesper.pedersen@redhat.com>"

LABEL summary="PostgreSQL 13 postgres_exporter" \
      description="PostgreSQL 13 postgres_exporter"

# COPY conf/licenses /licenses

ENV PGVERSION="13"
ENV PGROOT="/usr/pgsql-${PGVERSION}"

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
 && rpm -Uvh https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm \
# && dnf -y update \
 && dnf -y install wget tar \
 && dnf -y clean all

COPY root/ /

RUN mkdir -p /usr/local/bin
RUN cd /tmp \
 && wget https://github.com/prometheus-community/postgres_exporter/releases/download/v0.9.0/postgres_exporter-0.9.0.linux-amd64.tar.gz \
 && tar xzf postgres_exporter-0.9.0.linux-amd64.tar.gz \
 && mv postgres_exporter-0.9.0.linux-amd64/postgres_exporter /usr/local/bin/postgres_exporter \
 && rm -Rf postgres_exporter-0.9.0.linux-amd64 postgres_exporter-0.9.0.linux-amd64.tar.gz \
 && chown daemon:daemon /usr/local/bin/postgres_exporter

EXPOSE 9187

USER 2

CMD ["/usr/bin/run-pgexporter"]
