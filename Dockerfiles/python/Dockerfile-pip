FROM python:3.9-slim

# install system dependencies
RUN apt-get update \
  && apt-get -y install netcat gcc \
  && apt-get clean

# add new user
RUN useradd -ms /bin/bash python
USER python

# create and change home dir
WORKDIR /home/python

# set env variables
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

# port to expose on
EXPOSE 8000

# create and cd into app directory
WORKDIR /home/python/app

# copy requirements first so that they are cached and rebuild only when there is a change in dependency file
COPY ./requirements.txt ./

# install requirements
RUN pip install --user -r requirements.txt

# copy rest of the application
COPY . ./
