# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
FROM ghcr.io/maziggy/bambuddy:daily@sha256:8a351825c7b95a89e54f6f9680cac19d4bd12df76229d0788fda66da4b05df98 AS builder
FROM ghcr.io/home-assistant/base-python:3.14-alpine3.23

LABEL org.opencontainers.image.title="Home Assistant Add-on: Bambuddy add-on (DAILY)"
LABEL org.opencontainers.image.description="A powerful, self-hosted command center for Bambu Lab — from one A1 to a 40-printer farm. Your Printers. No Cloud. Your Rules."
LABEL org.opencontainers.image.source="https://github.com/Spegeli/homeassistant-app-bambuddy"
LABEL org.opencontainers.image.licenses="MIT License"

WORKDIR /app

# Install system dependencies
RUN apk add --no-cache \
  build-base \
  ca-certificates \
  py3-opencv \
  curl \
  ffmpeg

# Install Python dependencies with cache mount
COPY --from=builder /app/requirements.txt ./
RUN sed -i 's/opencv-python-headless/# opencv-python-headless/' /app/requirements.txt
RUN --mount=type=cache,target=/root/.cache/pip \
  pip install --root-user-action=ignore -r requirements.txt

COPY --from=builder /app /app

# Patch network_utils to enumerate alias IPs via BusyBox-compatible text parsing
COPY patches/network_utils.py /app/backend/app/services/network_utils.py

ENV PYTHONUNBUFFERED=1
ENV DATA_DIR=/app/data
ENV LOG_DIR=/app/logs

# Copy root filesystem
COPY rootfs /
RUN chmod a+x /etc/services.d/bambuddy/run /etc/services.d/bambuddy/finish
