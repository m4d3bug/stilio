FROM python:3.7-slim

ENV PYTHONUNBUFFERED=1
ENV ROOT=/usr/src/app

WORKDIR ${ROOT}

COPY . ${ROOT}

RUN apt-get update \
    && apt-get install -y build-essential

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

RUN apt-get remove -y build-essential \
    && apt-get -y autoremove
