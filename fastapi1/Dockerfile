FROM tiangolo/uvicorn-gunicorn:python3.8

LABEL maintainer="Sebastian Ramirez <tiangolo@gmail.com>"

RUN python -m pip install --upgrade pip
RUN pip install --no-cache-dir fastapi
RUN pip install python-multipart
RUN pip install pytest requests

COPY ./app /app
