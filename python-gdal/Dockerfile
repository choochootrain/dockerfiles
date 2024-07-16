FROM python:3.12.4-slim-bookworm

# Disable cache for apt-get because reasons
RUN echo 'Acquire::http::Pipeline-Depth 0;\nAcquire::http::No-Cache true;\nAcquire::BrokenProxy true;\n' > /etc/apt/apt.conf.d/99fixbadproxy

# Install GDAL build dependencies
RUN set -ex \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
      curl \
      cmake \
      make \
      autoconf \
      automake \
      build-essential \
      ca-certificates \
      pkg-config \
      gcc \
      g++ \
      git \
      libcurl4-gnutls-dev \
      libsqlite3-dev \
      libproj-dev \
    && rm -rf /var/lib/apt/lists/*

# Install GDAL 3.9.0
ENV GDAL_GIT_HASH bb5ea618e23e498e79e8b8e6bd7215ad94bb2103
RUN set -ex \
    && mkdir -p /usr/src \
    && cd /usr/src \
    && git clone https://github.com/OSGeo/gdal.git \
    && cd gdal \
    && git checkout ${GDAL_GIT_HASH} \
    && mkdir build \
    && cd build \
    && cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF -DBUILD_DOCS=OFF \
        -DGDAL_HIDE_INTERNAL_SYMBOLS=ON \
        -DRENAME_INTERNAL_TIFF_SYMBOLS=ON \
        -DGDAL_USE_CURL=ON \
        -DGDAL_USE_SQLITE3=ON \
        \
        -DGDAL_USE_ECW=OFF \
        -DGDAL_USE_GEOTIFF=OFF \
        -DGDAL_USE_HEIF=OFF \
        -DGDAL_USE_SPATIALITE=OFF \
    && make -j$(nproc) \
    && make install \
    && cd / \
    && rm -fr /usr/src/gdal

# Install Poetry 1.8.3
ENV POETRY_VERSION=1.8.3 \
    POETRY_HOME="/opt/poetry" \
    POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_VIRTUALENVS_CREATE=false
RUN curl -sSL https://install.python-poetry.org | python - --version $POETRY_VERSION
ENV PATH="$POETRY_HOME/bin:$PATH"
