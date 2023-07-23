# Use the official Python base image
FROM python:3.9

# Install GStreamer and its dependencies
RUN apt-get update && apt-get install -y \
    gstreamer1.0-plugins-base \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-libav \
    libgirepository1.0-dev \
    python3-gst-1.0 \
    python3-gi

RUN pip3 install pygobject

# Check whether GStreamer bindings were successfully installed
RUN python3 -c "import gi; gi.require_version('Gst', '1.0'); \
    gi.require_version('GstApp', '1.0'); \
    gi.require_version('GstVideo', '1.0'); \
    gi.require_version('GstBase', '1.0')"
