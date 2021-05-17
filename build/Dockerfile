FROM python:3.8.8-buster as installer

RUN apt-get update && \
    apt-get -y --no-install-recommends install \
    gettext \
    gcc \
    g++ \
    curl

ARG POETRY_VERSION=1.1.5
ENV PATH /root/.poetry/bin:$PATH
RUN pip install --upgrade pip && \
    curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -o get-poetry.py && \
    python get-poetry.py --version $POETRY_VERSION
ENV POETRY_VIRTUALENVS_CREATE="false"
ENV POETRY_INSTALLER_PARALLEL="false"
WORKDIR /home/interview

ENV PATH /home/interview/.venv/bin:$PATH

RUN python -m venv /home/interview/.venv && \
    pip install --upgrade pip

COPY pyproject.toml .

RUN poetry install --no-root

FROM python:3.8.8-slim-buster

WORKDIR /home/interview

RUN useradd -m -U -d /home/interview interview

COPY --from=installer --chown=interview:interview /home/interview/.venv /home/interview/.venv
ENV PATH /home/interview/.venv/bin:$PATH

RUN chown -R interview:interview /home/interview
USER interview
COPY project/* .
