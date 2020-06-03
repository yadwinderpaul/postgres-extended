FROM postgres:10.12
MAINTAINER Yadwinder Paul Singh <yadwinderpaul@gmail.com>

ENV POSTGIS_MAJOR 2.5
ENV PGTAP_VERSION v0.95.0

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    build-essential curl ca-certificates git gnupg

RUN curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    postgresql-server-dev-$PG_MAJOR \
    postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR \
    postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR-scripts && \
  rm -rf /var/lib/apt/lists/*

RUN curl -LO http://xrl.us/cpanm && \
  chmod +x cpanm && \
  ./cpanm TAP::Parser::SourceHandler::pgTAP

RUN git clone git://github.com/theory/pgtap.git && \
  cd pgtap && \
  git checkout tags/$PGTAP_VERSION

RUN mkdir -p /usr/share/doc/postgresql-doc-$PG_MAJOR
RUN mkdir -p /usr/share/postgresql/10.12/extension
RUN chown postgres:postgres -R /pgtap
RUN chown postgres:postgres -R /usr/share/doc/postgresql-doc-$PG_MAJOR
RUN chown postgres:postgres -R /usr/share/postgresql

# install locales
RUN apt-get install --reinstall -y locales
RUN sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i 's/# sv_SE.UTF-8 UTF-8/sv_SE.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i 's/# fr_FR.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i 's/# nl_NL.UTF-8 UTF-8/nl_NL.UTF-8 UTF-8/' /etc/locale.gen
RUN locale-gen en_US.UTF-8 sv_SE.UTF-8 fr_FR.UTF-8 nl_NL.UTF-8

RUN mkdir -p /docker-entrypoint-initdb.d
COPY ./initdb-pgtap.sh /docker-entrypoint-initdb.d/pgtap.sh
