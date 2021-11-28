FROM python:2.7-onbuild
ENV PYTHONPATH /usr/src/app
WORKDIR /app
COPY consumer.py /app
ADD requirements.txt /app
RUN pip install --requirement /app/requirements.txt
