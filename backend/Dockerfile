FROM python:3.7.7-slim

WORKDIR /work
RUN apt-get update && \
    apt-get install -y gcc && \
    pip install --no-cache-dir pipenv
COPY ./ ./
RUN pipenv install --skip-lock --system && \
    rm -f Pipfile

EXPOSE 8001
CMD ["uwsgi", "uwsgi.ini"]
