FROM postgres:9.6
MAINTAINER Yadwinder Paul Singh <yadwinderpaul@gmail.com>

ENV POSTGIS_MAJOR 2.5
ENV POSTGIS_VERSION 2.5.0+dfsg-2.pgdg90+1
ENV PGTAP_VERSION v0.95.0

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    build-essential curl ca-certificates git postgresql-server-dev-$PG_MAJOR \
    postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR=$POSTGIS_VERSION \
    postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR-scripts=$POSTGIS_VERSION \
    postgis=$POSTGIS_VERSION && \
  rm -rf /var/lib/apt/lists/*

RUN curl -LO http://xrl.us/cpanm && \
  chmod +x cpanm && \
  ./cpanm TAP::Parser::SourceHandler::pgTAP

RUN git clone git://github.com/theory/pgtap.git && \
  cd pgtap && \
  git checkout tags/$PGTAP_VERSION

RUN mkdir -p /usr/share/doc/postgresql-doc-$PG_MAJOR
RUN mkdir -p /usr/share/postgresql/9.6/extension
RUN chown postgres:postgres -R /pgtap
RUN chown postgres:postgres -R /usr/share/doc/postgresql-doc-$PG_MAJOR
RUN chown postgres:postgres -R /usr/share/postgresql

RUN mkdir -p /docker-entrypoint-initdb.d
COPY ./initdb-pgtap.sh /docker-entrypoint-initdb.d/pgtap.sh
