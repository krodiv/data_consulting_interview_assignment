FROM python:3.10-slim as builder

# pin version of poetry
RUN pip install poetry==1.4.0

# add dependancies for psycopg2
RUN apt-get update \
    && apt-get -y install libpq-dev gcc \
    && pip install psycopg2

# poetry configuration for build
ENV POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_VIRTUALENVS_CREATE=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache

WORKDIR /market_pipeline

# set dependencies from pyproject.toml and poetry.lock
COPY pyproject.toml poetry.lock ./
RUN touch README.md

# poetry config for not using virtualenv inside docker container
RUN poetry config virtualenvs.create false
# ensure no dev tools are installed in container
RUN poetry install --no-dev

COPY market_pipeline ./market_pipeline

CMD source /market_pipeline/.venv/bin/activate
ENTRYPOINT ["poetry", "run", "python", "-m", "market_pipeline.app"]