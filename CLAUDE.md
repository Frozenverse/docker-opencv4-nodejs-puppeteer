# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository builds a Docker image combining Ubuntu 22.04, Node.js 22, OpenCV 4.13, and Puppeteer dependencies. The image is published to Docker Hub as `frozenverse/node22-puppeteer-opencv4:latest`.

## Build Commands

Build the Docker image locally:
```bash
docker build -t node22-puppeteer-opencv4 ubuntu22.04/
```

Build with custom versions:
```bash
docker build --build-arg NODE_VERSION=22 --build-arg OPENCV_VERSION=4.13.0 -t node22-puppeteer-opencv4 ubuntu22.04/
```

## Architecture

- `ubuntu22.04/Dockerfile` - Single Dockerfile that installs:
  - Build prerequisites (cmake, g++, etc.)
  - Puppeteer browser dependencies (X11, GTK, fonts, etc.)
  - Node.js from NodeSource
  - OpenCV with contrib modules compiled from source

## CI/CD

GitHub Actions workflow (`.github/workflows/build.yml`) automatically builds and pushes to Docker Hub on pushes to `main`. Requires `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` secrets.
