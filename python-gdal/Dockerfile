FROM andrejreznik/python-gdal:py3.12.4-gdal3.9.1

# Install Poetry 1.8.3
ENV POETRY_VERSION=1.8.3 \
    POETRY_HOME="/opt/poetry" \
    POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_VIRTUALENVS_CREATE=false
RUN curl -sSL https://install.python-poetry.org | python - --version $POETRY_VERSION
ENV PATH="$POETRY_HOME/bin:$PATH"

# Initialize virtualenv
ENV VIRTUAL_ENV="/opt/venv"
RUN python -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV:$PATH"

# Disable cache for apt-get
RUN echo 'Acquire::http::Pipeline-Depth 0;\nAcquire::http::No-Cache true;\nAcquire::BrokenProxy true;\n' > /etc/apt/apt.conf.d/99fixbadproxy

# Install other build dependencies idk
RUN set -ex \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        g++ \
        libgeos-dev \
    && rm -rf /var/lib/apt/lists/*
