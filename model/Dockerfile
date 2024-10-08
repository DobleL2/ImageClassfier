# Stage 1: Base environment
FROM condaforge/miniforge3 as base

# Set environment variable to avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies for h5py and other TensorFlow libraries
RUN apt-get update && apt-get install -y \
    build-essential \
    pkg-config \
    libhdf5-dev \
    libhdf5-serial-dev \
    hdf5-tools \
    zlib1g-dev \
    libssl-dev \
    libffi-dev \
    libjpeg-dev \
    liblapack-dev \
    libblas-dev \
    gfortran \
    python3-dev \
    && apt-get clean

# Set up paths for the Python environment
ENV PYTHONPATH=$PYTHONPATH:/src/

# Install Python dependencies including TensorFlow
ADD requirements.txt .
RUN pip3 install --upgrade pip && \
    pip3 install -r requirements.txt

# Copy application code
COPY ./ /src/
WORKDIR /src

# Stage 2: Test environment
FROM base as test

# Run pytest for the tests in the /src/tests directory
RUN ["pytest", "-v", "/src/tests"]

# Stage 3: Build/Production environment
FROM base as build

# Expose any required ports (optional, for serving via web app if needed)
EXPOSE 8000

# Set entry point for running the ML service
ENTRYPOINT ["python3", "/src/ml_service.py"]


# FROM python:3.8.13 as base


# # Append /home/app/.local/bin/ to PATH variable because
# # gunicorn is installed there.
# ENV PYTHONPATH=$PYTHONPATH:/src/

# ADD requirements.txt .
# RUN pip3 install -r requirements.txt

# ENV PYTHONPATH=$PYTHONPATH:/src/

# COPY ./ /src/

# WORKDIR /src

# FROM base as test
# RUN ["pytest", "-v", "/src/tests"]

# FROM base as build
# ENTRYPOINT ["python3", "/src/ml_service.py"]

