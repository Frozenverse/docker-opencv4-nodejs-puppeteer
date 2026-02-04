# Stage 1: Build OpenCV
FROM ubuntu:22.04 AS builder

ARG OPENCV_VERSION=4.13.0

# Install build dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    g++ \
    wget \
    unzip \
    libgtk-3-dev \
    libpng-dev \
    libjpeg-dev

# Download, build, and install OpenCV (clean up in same layer)
RUN mkdir -p /opencv && \
    cd /opencv && \
    wget -O opencv.zip https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
    wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip && \
    unzip opencv.zip && \
    unzip opencv_contrib.zip && \
    mkdir -p build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib-${OPENCV_VERSION}/modules \
          -D BUILD_EXAMPLES=OFF ../opencv-${OPENCV_VERSION} && \
    cmake --build . --config Release -j$(nproc) && \
    make install && \
    cd / && rm -rf /opencv

# Stage 2: Runtime image
FROM ubuntu:22.04

ARG NODE_VERSION=22

# Install Puppeteer Dependencies and OpenCV runtime dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    wget \
    fonts-liberation \
    gconf-service \
    libappindicator1 \
    libasound2 \
    libatk1.0-0 \
    libcairo2 \
    libcups2 \
    libfontconfig1 \
    libgbm-dev \
    libgdk-pixbuf2.0-0 \
    libgtk-3-0 \
    libicu-dev \
    libjpeg-dev \
    libnspr4 \
    libnss3 \
    libpango-1.0-0 \
    libpangocairo-1.0-0 \
    libpng-dev \
    libx11-6 \
    libx11-xcb1 \
    libxcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxi6 \
    libxrandr2 \
    libxrender1 \
    libxss1 \
    libxtst6 \
    xdg-utils

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - && \
    apt-get install -y nodejs

# Copy OpenCV from builder stage
COPY --from=builder /usr/local /usr/local

# Update library cache
RUN ldconfig
