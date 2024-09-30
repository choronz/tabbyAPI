FROM nvidia/cuda:12.4.1-devel-ubuntu22.04
#FROM nvidia/cuda:12.4.1-runtime-ubuntu22.04

# Install Python and other dependencies   ppa:deadsnakes/ppa
RUN echo 'tzdata tzdata/Areas select America' | debconf-set-selections \
    && echo 'tzdata tzdata/Zones/America select Los_Angeles' | debconf-set-selections ca-certificates \
    && apt-get update -y \
    && apt-get install -y ccache software-properties-common git curl build-essential ca-certificates \
    && add-apt-repository ppa:fkrull/deadsnakes \
    && apt-get update -y \
    && apt-get install -y python${PYTHON_VERSION} python${PYTHON_VERSION}-dev python${PYTHON_VERSION}-venv \
    && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python${PYTHON_VERSION} 1 \
    && update-alternatives --set python3 /usr/bin/python${PYTHON_VERSION} \
    && ln -sf /usr/bin/python${PYTHON_VERSION}-config /usr/bin/python3-config \
    && curl -sS https://bootstrap.pypa.io/get-pip.py | python${PYTHON_VERSION} \
    && python3 --version && python3 -m pip --version
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
