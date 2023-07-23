# Use the official Python base image
FROM python:3.9

# Install GStreamer and its dependencies
RUN apt-get update && apt-get install -y \
    libgstreamer1.0-0 \
    gstreamer1.0-plugins-base \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-libav \
    python3-gi \
    gir1.2-gstreamer-1.0 \
    gir1.2-gst-plugins-base-1.0

# Set the working directory inside the container
WORKDIR /app

# Copy the server files to the container's working directory
COPY / /app/server/

# Install the server dependencies
COPY server/requirements.txt /app/server/
RUN pip install --no-cache-dir -r /app/server/requirements.txt

# Expose the port that the server will be listening on
EXPOSE 8765

# Start the server using the app.py file (modify if your entry point is different)
CMD ["python", "/app/server/app.py"]
