# Use Debian Bullseye (more up-to-date than Buster)
FROM python:3.9-slim-bullseye

# Install system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        openjdk-11-jre-headless \
        wget \
        gcc \
        g++ \
        make \
        build-essential \
        iputils-ping \
        net-tools \
        librdkafka-dev \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy requirements.txt first to leverage Docker cache
COPY requirements.txt /app/requirements.txt

# Install Python dependencies
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir -r /app/requirements.txt

# Copy wait-for-it.sh and set permissions
COPY wait-for-it.sh /wait-for-it.sh
RUN chmod +x /wait-for-it.sh

# Copy Kafka producer script
COPY kafka_producer.py /app/kafka_producer.py

# Default command
CMD ["/wait-for-it.sh", "kafka_broker:29092", "--", "python3", "/app/kafka_producer.py"]
