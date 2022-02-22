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
# pgsql14-grafana-rocky8
FROM rockylinux:8

LABEL maintainer="Jesper Pedersen <jesper.pedersen@redhat.com>"

LABEL summary="PostgreSQL 14 Grafana" \
      description="PostgreSQL 14 Grafana"

# COPY conf/licenses /licenses

ENV PGVERSION="14"
ENV PGROOT="/usr/pgsql-${PGVERSION}"

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
 && rpm -Uvh https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm \
 && dnf -y update \
 && dnf -y module disable postgresql \
 && dnf -y install wget git gcc make tar redhat-rpm-config clang ccache llvm llvm-static \
 && dnf -y install postgresql14 postgresql14-server postgresql14-devel postgresql14-contrib postgresql14-libs \
 && dnf -y install timescaledb_14 \
 && dnf -y clean all

COPY root/ /

RUN mkdir -p /conf /pgconf /pgdata /pgwal
COPY conf/* /conf/
RUN chown -R postgres:postgres /conf /pgconf /pgdata /pgwal
RUN chmod 700 /conf /pgconf /pgdata /pgwal

RUN cd /tmp \
 && git clone https://github.com/timescale/pg_prometheus.git \
 && cd pg_prometheus \
 && export PATH=/usr/pgsql-14/bin:$PATH \
 && make \
 && make install \
 && cd .. \
 && rm -Rf pg_prometheus

RUN cd /tmp \
 && wget https://github.com/timescale/prometheus-postgresql-adapter/releases/download/v0.6.0/prometheus-postgresql-adapter-0.6.0-linux-amd64.tar.gz \
 && tar xzf prometheus-postgresql-adapter-0.6.0-linux-amd64.tar.gz \
 && mv prometheus-postgresql-adapter /usr/local/bin/prometheus-postgresql-adapter \
 && rm -Rf prometheus-postgresql-adapter-0.6.0-linux-amd64.tar.gz \
 && chown postgres:postgres /usr/local/bin/prometheus-postgresql-adapter

RUN cd /tmp \
 && wget https://github.com/prometheus/prometheus/releases/download/v2.33.3/prometheus-2.33.3.linux-amd64.tar.gz \
 && mkdir -p /usr/local/ \
 && cd /usr/local \
 && tar xzf /tmp/prometheus-2.33.3.linux-amd64.tar.gz \
 && mv prometheus-2.33.3.linux-amd64 prometheus \
 && chown -R postgres:postgres prometheus \
 && rm -Rf /tmp/prometheus-2.33.3.linux-amd64.tar.gz

RUN cd /tmp \
 && wget https://dl.grafana.com/oss/release/grafana-8.4.1.linux-amd64.tar.gz \
 && mkdir -p /usr/local/ \
 && cd /usr/local \
 && tar xzf /tmp/grafana-8.4.1.linux-amd64.tar.gz \
 && mv grafana-8.4.1 grafana \
 && chown -R postgres:postgres grafana \
 && rm -Rf /tmp/grafana-8.4.1.linux-amd64.tar.gz

RUN dnf -y remove wget git gcc make tar redhat-rpm-config clang ccache llvm llvm-static \
 && dnf -y clean all

VOLUME ["/pgconf", "/pgdata", "/pgwal"]

EXPOSE 9090
EXPOSE 3000

USER 26

CMD ["/usr/bin/run-grafana"]
