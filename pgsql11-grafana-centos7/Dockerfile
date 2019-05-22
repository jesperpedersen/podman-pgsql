# The MIT License (MIT)
#
# Copyright (c) 2019 Jesper Pedersen <jesper.pedersen@redhat.com>
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
# pgsql11-grafana-centos7
FROM centos:centos7

LABEL maintainer="Jesper Pedersen <jesper.pedersen@redhat.com>"

LABEL summary="PostgreSQL 11 Grafana" \
      description="PostgreSQL 11 Grafana"

# COPY conf/licenses /licenses

ENV PGVERSION="11"
ENV PGROOT="/usr/pgsql-${PGVERSION}"

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
 && rpm -Uvh https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-centos11-11-2.noarch.rpm \
 && yum -y install centos-release-scl-rh \
 && yum -y install wget tar git gcc llvm5.0 llvm5.0-static make \
 && yum -y install devtoolset-7 llvm-toolset-7 \
 && yum -y update \
 && yum -y install postgresql11 postgresql11-server postgresql11-devel postgresql11-contrib postgresql11-libs \
 && yum install -y timescaledb_11 \
 && yum -y clean all

COPY root/ /

RUN mkdir -p /conf /pgconf /pgdata /pgwal
COPY conf/* /conf/
RUN chown -R postgres:postgres /conf /pgconf /pgdata /pgwal
RUN chmod 700 /conf /pgconf /pgdata /pgwal

RUN cd /tmp \
 && git clone https://github.com/timescale/pg_prometheus.git \
 && cd pg_prometheus \
 && export PATH=/usr/pgsql-11/bin:$PATH \
 && make \
 && make install \
 && cd .. \
 && rm -Rf pg_prometheus

RUN cd /tmp \
 && wget https://github.com/timescale/prometheus-postgresql-adapter/releases/download/0.4.1/prometheus-postgresql-adapter-0.4.1-linux-amd64.tar.gz \
 && tar xzf prometheus-postgresql-adapter-0.4.1-linux-amd64.tar.gz \
 && mv prometheus-postgresql-adapter /usr/local/bin/prometheus-postgresql-adapter \
 && rm -Rf prometheus-postgresql-adapter-0.4.1-linux-amd64.tar.gz \
 && chown postgres:postgres /usr/local/bin/prometheus-postgresql-adapter

RUN cd /tmp \
 && wget https://github.com/prometheus/prometheus/releases/download/v2.9.2/prometheus-2.9.2.linux-amd64.tar.gz \
 && mkdir -p /usr/local/ \
 && cd /usr/local \
 && tar xzf /tmp/prometheus-2.9.2.linux-amd64.tar.gz \
 && mv prometheus-2.9.2.linux-amd64 prometheus \
 && chown -R postgres:postgres prometheus \
 && rm -Rf /tmp/prometheus-2.9.2.linux-amd64.tar.gz

RUN cd /tmp \
 && wget https://dl.grafana.com/oss/release/grafana-6.2.0.linux-amd64.tar.gz \
 && mkdir -p /usr/local/ \
 && cd /usr/local \
 && tar xzf /tmp/grafana-6.2.0.linux-amd64.tar.gz \
 && mv grafana-6.2.0 grafana \
 && chown -R postgres:postgres grafana \
 && rm -Rf /tmp/grafana-6.2.0.linux-amd64.tar.gz

VOLUME ["/pgconf", "/pgdata", "/pgwal"]

EXPOSE 9090
EXPOSE 3000

USER 26

CMD ["/usr/bin/run-grafana"]
