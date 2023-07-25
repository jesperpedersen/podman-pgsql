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
# pgsql14-pgmoneta-rocky8
FROM rockylinux:8

LABEL maintainer="Jesper Pedersen <jesper.pedersen@redhat.com>"

LABEL summary="PostgreSQL 14 pgmoneta" \
      description="PostgreSQL 14 pgmoneta"

# COPY conf/licenses /licenses

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
 && rpm -Uvh https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm \
 && dnf -y update \
 && dnf -y module disable postgresql \
 && dnf -y install wget tar crontabs \
 && dnf -y install postgresql14 postgresql14-contrib postgresql14-libs \
 && dnf -y install pgmoneta \
 && dnf -y clean all

RUN useradd -ms /bin/bash pgmoneta

COPY root/ /

RUN mkdir -p /conf /pgconf /pgmoneta
COPY conf/* /conf/
RUN chown -R pgmoneta:pgmoneta /conf /pgconf /pgmoneta
RUN chmod 700 /conf /pgconf /pgmoneta

RUN mkdir -p /usr/local/bin
RUN cd /tmp \
 && wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz \
 && tar xzf node_exporter-1.6.1.linux-amd64.tar.gz \
 && mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/node_exporter \
 && rm -Rf node_exporter-1.6.1.linux-amd64 node_exporter-1.6.1.linux-amd64.tar.gz \
 && chown pgmoneta:pgmoneta /usr/local/bin/node_exporter

VOLUME ["/pgconf", "/pgmoneta"]

EXPOSE 5001
EXPOSE 9100

USER pgmoneta
WORKDIR /home/pgmoneta

CMD ["/usr/bin/run-pgmoneta"]
