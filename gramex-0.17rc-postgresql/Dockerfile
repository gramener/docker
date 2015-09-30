# This Dockerfile installs Gramex 0.17rc and PostgreSQL

FROM ubuntu:14.04

MAINTAINER Suhas SG <suhas@gramener.com>

RUN apt-get update
RUN apt-get -y install sudo
RUN apt-get -y install unzip

# Install Anaconda
# ----------------
# From https://github.com/ContinuumIO/docker-images/blob/master/anaconda/Dockerfile

RUN apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1
RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda-2.3.0-Linux-x86_64.sh && \
    /bin/bash /Anaconda-2.3.0-Linux-x86_64.sh -b -p /opt/conda && \
    rm /Anaconda-2.3.0-Linux-x86_64.sh && \
    /opt/conda/bin/conda install --yes conda==3.14.1

ENV PATH /opt/conda/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# Install PostgreSQL
# ------------------
# From https://github.com/docker/docker/blob/master/docs/examples/postgresql_service.Dockerfile

# Add the PostgreSQL PGP key to verify their Debian packages.
# It should be the same key as https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

# Add PostgreSQL's repository. It contains the most recent stable release
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN apt-get update &&\
    apt-get install -y python-software-properties software-properties-common postgresql-9.4 postgresql-client-9.4 postgresql-contrib-9.4 postgresql-server-dev-9.4

# Run the rest of the commands as the ``postgres`` user created by the ``postgres-9.4`` package when it was ``apt-get installed``
USER postgres

# Create a PostgreSQL role named ``gramex`` with ``gramex`` as the password and
# then create a database `gramex` owned by the ``gramex`` role.
# Note: here we use ``&&\` to run commands one after the other - the ``\`
#       allows the RUN command to span multiple lines.
RUN /etc/init.d/postgresql start &&\
    psql --command "CREATE USER gramex WITH SUPERUSER PASSWORD 'gramex';" &&\
    createdb -O gramex gramex

# Adjust PostgreSQL configuration so that remote connections to the
# database are possible.
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.4/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.4/main/postgresql.conf

# Add VOLUMEs to allow backup of config, logs and databases
VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

RUN service postgresql start


# Install gramex
# --------------

USER root
RUN /opt/conda/bin/pip install Markdown redis python-dateutil rsa backports.ssl_match_hostname certifi psycopg2
RUN useradd -m gramex && echo "gramex:gramex" | chpasswd && adduser gramex sudo

USER gramex
WORKDIR "/home/gramex"
RUN wget --quiet https://learn.gramener.com/downloads/release/gramex-0.17.0rc.zip -O gramex.zip
RUN unzip gramex.zip -d gramex

# Expose PostgreSQL, default Gramex ports
EXPOSE 80 5432 8888 8889 8890
