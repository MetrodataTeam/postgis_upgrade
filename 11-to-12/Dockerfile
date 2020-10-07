FROM metrodatatech/mdtdb:12-full

RUN sed -i 's/$/ 11/' /etc/apt/sources.list.d/pgdg.list
ADD https://packages.groonga.org/debian/groonga-archive-keyring-latest-buster.deb /tmp/groonga-archive-keyring-latest-buster.deb
RUN apt-get update \
    && apt-cache showpkg postgresql-11-postgis-$POSTGIS_MAJOR \
    && apt-get install -y --no-install-recommends \
        postgresql-11=11.9-0+deb10u1 \
        postgresql-11-postgis-2.5 \
        postgresql-11-postgis-2.5-scripts \
    && cp /usr/lib/postgresql/12/lib/postgis-3.so /usr/lib/postgresql/12/lib/postgis-2.5.so \
    && cp /usr/lib/postgresql/12/lib/postgis_raster-3.so /usr/lib/postgresql/12/lib/postgis_raster-2.5.so \
    && cp /usr/lib/postgresql/12/lib/postgis_topology-3.so /usr/lib/postgresql/12/lib/postgis_topology-2.5.so \
    && apt-get install -y -V /tmp/groonga-archive-keyring-latest-buster.deb \
    && apt-get update \
    && apt-get install -y --no-install-recommends postgresql-11-pgroonga \
    && rm -rf /var/lib/apt/lists/*

ENV PGBINOLD /usr/lib/postgresql/11/bin
ENV PGBINNEW /usr/lib/postgresql/12/bin

ENV PGDATAOLD /var/lib/postgresql/11/data
ENV PGDATANEW /var/lib/postgresql/12/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
	&& chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade.sh /usr/local/bin/

ENTRYPOINT ["docker-upgrade.sh"]

# recommended: --link
CMD ["pg_upgrade"]
