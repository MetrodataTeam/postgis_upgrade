FROM metrodatatech/mdtdb:12-full

RUN sed -i 's/$/ 11/' /etc/apt/sources.list.d/pgdg.list
ENV POSTGIS_MAJOR 2.5
ENV POSTGIS_VERSION 2.5.1+dfsg-1

RUN apt-get update \
    && apt-cache showpkg postgresql-11-postgis-$POSTGIS_MAJOR \
    && apt-get install -y --no-install-recommends \
        postgresql-11=11.9-0+deb10u1 \
        postgresql-11-postgis-$POSTGIS_MAJOR=$POSTGIS_VERSION \
        postgresql-11-postgis-$POSTGIS_MAJOR-scripts=$POSTGIS_VERSION \
    && rm -rf /var/lib/apt/lists/*

ENV PGBINOLD /usr/lib/postgresql/11/bin
ENV PGBINNEW /usr/lib/postgresql/12/bin

ENV PGDATAOLD /var/lib/postgresql/11/data
ENV PGDATANEW /var/lib/postgresql/12/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
	&& chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade /usr/local/bin/

ENTRYPOINT ["docker-upgrade"]

# recommended: --link
CMD ["pg_upgrade"]
