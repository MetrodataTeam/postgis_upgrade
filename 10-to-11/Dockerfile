FROM mdillon/postgis:11

RUN sed -i 's/$/ 10/' /etc/apt/sources.list.d/pgdg.list

RUN apt-get update \
    && apt-cache showpkg postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR \
    && apt-get install -y --no-install-recommends \
            postgresql-10=10.6-1.pgdg90+1 \
            postgresql-10-postgis-$POSTGIS_MAJOR=$POSTGIS_VERSION \
            postgresql-10-postgis-$POSTGIS_MAJOR-scripts=$POSTGIS_VERSION \
            postgis=$POSTGIS_VERSION \
          && rm -rf /var/lib/apt/lists/* \
    # maybe remove this hard coded part
    && localedef -f UTF-8 -i zh_CN zh_CN.UTF-8


ENV PGBINOLD /usr/lib/postgresql/10/bin
ENV PGBINNEW /usr/lib/postgresql/11/bin

ENV PGDATAOLD /var/lib/postgresql/10/data
ENV PGDATANEW /var/lib/postgresql/11/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
    && chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade.sh /usr/local/bin/

ENTRYPOINT ["docker-upgrade.sh"]

# recommended: --link
CMD ["pg_upgrade"]
