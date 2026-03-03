FROM docker.osgeo.org/geoserver:stable-latest-gdal

# Install wget (required for PostgreSQL cert)
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p ~/.postgresql && \
    wget "https://storage.yandexcloud.net/cloud-certs/CA.pem" \
        --output-document ~/.postgresql/root.crt && \
    chmod 0655 ~/.postgresql/root.crt