FROM python:3.9-slim

ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

# install system dependencies
RUN apt-get update \
  && apt-get -y install netcat gcc curl\
  && apt-get clean

# add new user
RUN useradd -ms /bin/bash python
USER python

# create and change home dir
WORKDIR /home/python

# port to expose on
EXPOSE 8000

# Install poetry
RUN curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python
ENV PATH="${PATH}:/root/.poetry/bin"

# change working directory
WORKDIR /home/python/app

# copy python dependency file
COPY ./pyproject.toml ./pyproject.toml

# install dependency using poetry -- caching
RUN poetry config virtualenvs.create false \
  && poetry install --no-interaction --no-ansi

# copy rest of the project files
COPY . ./

# Run  Static Analysis
RUN poetry add flake8 pep8-naming flake8-print
RUN flake8 --max-line-length=130 app
