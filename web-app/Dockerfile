FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt /tmp

RUN pip install --no-cache-dir -r /tmp/requirements.txt && rm /tmp/requirements.txt

RUN apt update && apt install --no-install-recommends -y curl\
    && rm -rf /var/lib/apt/lists/*

RUN groupadd -r test-app && useradd --no-log-init -r -g test-app test-app

COPY ./src /app

RUN chown -R test-app:test-app /app

USER test-app

EXPOSE 5000

HEALTHCHECK --interval=10s --timeout=5s --retries=3 CMD curl --fail http://localhost:5000 || exit 1

CMD ["flask", "run", "--host=0.0.0.0"]
