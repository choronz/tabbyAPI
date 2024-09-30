FROM nvidia/cuda:12.4.1-runtime-ubuntu22.04

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common \
    build-essential \
    curl \
    ca-certificates \
    python3.12 \
    python3.12-dev\
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip3 install --no-cache-dir --upgrade pip

# Set the working directory in the container
WORKDIR /app

# Get requirements
COPY pyproject.toml .

# Install packages specified in pyproject.toml cu124
RUN pip3 install --no-cache-dir .[cu124]

RUN rm pyproject.toml

# Copy the current directory contents into the container
COPY . .

# Make port 5000 available to the world outside this container
EXPOSE 8000

# Set the entry point
ENTRYPOINT ["python3"]

# Run main.py when the container launches
CMD ["main.py"]
