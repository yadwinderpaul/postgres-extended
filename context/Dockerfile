FROM postgres:12.2
MAINTAINER Yadwinder Paul Singh <yadwinderpaul@gmail.com>

ENV POSTGIS_MAJOR 2.5
ENV PGTAP_VERSION v1.1.0

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


# install pgtap
RUN curl -LO http://xrl.us/cpanm && \
  chmod +x cpanm && \
  ./cpanm TAP::Parser::SourceHandler::pgTAP

RUN git clone git://github.com/theory/pgtap.git && \
  cd pgtap && \
  git checkout tags/$PGTAP_VERSION

RUN mkdir -p /usr/share/doc/postgresql-doc-$PG_MAJOR
RUN mkdir -p /usr/share/postgresql/$PG_MAJOR/extension
RUN chown postgres:postgres -R /pgtap
RUN chown postgres:postgres -R /usr/share/doc/postgresql-doc-$PG_MAJOR
RUN chown postgres:postgres -R /usr/share/postgresql

WORKDIR "/pgtap"
RUN make
RUN make install
WORKDIR "/"


# install locales
RUN apt-get install --reinstall -y locales
RUN sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i 's/# sv_SE.UTF-8 UTF-8/sv_SE.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i 's/# fr_FR.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i 's/# nl_NL.UTF-8 UTF-8/nl_NL.UTF-8 UTF-8/' /etc/locale.gen
RUN locale-gen en_US.UTF-8 sv_SE.UTF-8 fr_FR.UTF-8 nl_NL.UTF-8
