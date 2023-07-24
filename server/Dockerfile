FROM ubuntu:22.10 as gst-plugins

ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && \
    apt-get install -y \
      git \
      pkg-config \
      curl \
      build-essential \
      libssl-dev \
      libglib2.0-dev \
      gstreamer1.0 \
      libgstreamer-plugins-base1.0-dev \
      libgstreamer-plugins-bad1.0-dev

# Install Rust, Cargo and Cargo C-ABI helpers
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN cargo install cargo-c

WORKDIR /src

# Build gst-plugins-rs plugins
RUN git clone https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs.git && \
    cd gst-plugins-rs && \
    cargo cbuild -p gst-plugin-webrtc

# Check whether webrtc plugins were successfully installed
RUN GST_PLUGIN_PATH="$(echo -n target/*-unknown-linux-gnu/debug):$GST_PLUGIN_PATH" gst-inspect-1.0 webrtcsink

# Use the official Python base image
FROM python:3.9

# Install GStreamer and its dependencies
RUN apt-get update && apt-get install -y \
    gstreamer1.0-plugins-base \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-libav \
    gstreamer1.0 \
    libgirepository1.0-dev \
    python3-gst-1.0 \
    python3-gi

WORKDIR /code/

# Install application dependencies
COPY requirements.txt ./
RUN pip3 install -r requirements.txt

# Check whether GStreamer bindings were successfully installed
RUN python3 -c "import gi; \
    gi.require_version('Gst', '1.0'); \
    gi.require_version('GstApp', '1.0'); \
    gi.require_version('GstVideo', '1.0'); \
    gi.require_version('GstBase', '1.0')"

# Copy compiled GStreamer plugins
COPY --from=gst-plugins /code/gst-plugins-rs/target/* /opt/gst-plugins-rs/
ENV GST_PLUGIN_PATH="/opt/gst-plugins-rs/debug:$GST_PLUGIN_PATH"

# Check whether GStreamer was successfully installed
RUN gst-inspect-1.0 webrtcsink
