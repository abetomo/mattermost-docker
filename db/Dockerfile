FROM postgres:13.2-alpine

ENV DEFAULT_TIMEZONE UTC
# Mandatory nowdays with PG 10
ENV WAL_LEVEL logical

# Install some packages to use WAL
RUN apk add --no-cache \
      build-base \
      curl \
      libc6-compat \
      libffi-dev \
      linux-headers \
      python3-dev \
      py-pip \
      py-cryptography \
      pv \
    && pip install --upgrade pip \
    && pip --no-cache-dir install wal-e \
    && rm -rf /var/cache/apk/* /tmp/* /var/tmp/*

# Add wale script
COPY setup-wale.sh /docker-entrypoint-initdb.d/

# Healthcheck to make sure container is ready
HEALTHCHECK CMD pg_isready -U $POSTGRES_USER -d $POSTGRES_DB || exit 1

# Add and configure entrypoint and command
COPY entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["postgres"]

VOLUME ["/var/run/postgresql", "/usr/share/postgresql/", "/var/lib/postgresql/data", "/tmp", "/etc/wal-e.d/env"]
