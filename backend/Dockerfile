# Pin the Python base image for all stages and
# install all shared dependencies.
#FROM python:3-slim AS base
FROM python:3.12-slim AS base


# Tweak Python to run better in Docker
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=on

# Install the Poetry package manager, asking it
# to create virtual environments in the project directory.
ENV POETRY_VERSION=2.5.1 \
    POETRY_VIRTUALENVS_CREATE=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache

RUN --mount=type=cache,target=/root/.cache/pip \
    pip install poetry==$POETRY_VERSION

WORKDIR /app

COPY . .

RUN poetry install 

# Set up the database with the necessary tables
#RUN poetry run bash ./prestart.sh

# RUN --mount=type=secret,id=app_env,required=true \
#     set -a && . /run/secrets/app_env && \
#     poetry run bash ./prestart.sh

# Run the backend server
#CMD ["poetry", "run", "uvicorn", "app.main:app", "--reload"]

CMD ["sh", "-c", "poetry run bash ./prestart.sh && exec poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]