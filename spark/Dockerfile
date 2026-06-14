# Use Debian Bullseye (more up-to-date than Buster)
FROM python:3.9-slim-bullseye

# Install system dependencies for Spark and Java
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        openjdk-11-jre-headless \
        wget \
        gcc \
        g++ \
        make \
        build-essential \
    && rm -rf /var/lib/apt/lists/*

# Set Spark and Hadoop versions
ENV SPARK_VERSION=3.5.0
ENV HADOOP_VERSION=3

# Download and install Apache Spark
RUN wget https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
    tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
    mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} /opt/spark && \
    rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

# Set environment variables for Spark
ENV SPARK_HOME=/opt/spark
ENV PATH=$PATH:$SPARK_HOME/bin

# Set working directory
WORKDIR /app

# Copy requirements.txt first to leverage Docker cache
COPY requirements.txt /app/requirements.txt

# Install Python dependencies
RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir -r /app/requirements.txt

# Copy the rest of the application
COPY . /app/

# Default command to run the Spark processing script
CMD ["python3", "/app/process_data.py"]
