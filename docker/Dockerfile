# Use an official NVIDIA base image with CUDA support
FROM nvidia/cuda:12.1.1-cudnn8-devel-ubuntu22.04

# Set label for the docker image description
LABEL description="Docker image for xtts-api-server"

# Install required packages (avoid cache to reduce image size)
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    python3-dev portaudio19-dev libportaudio2 libasound2-dev libportaudiocpp0 \
    git python3 python3-pip make g++ ffmpeg && \
    rm -rf /var/lib/apt/lists/*

# Upgrade pip and install virtualenv
RUN python3 -m pip install --upgrade pip setuptools wheel ninja virtualenv

# Copy the application source code to /app directory and change the workdir to /app
# COPY . /app
# WORKDIR /app

# Install Python dependencies
# Copy requirements.txt to the container
COPY requirements.txt .

# Install PyTorch and torchaudio
RUN pip install torch torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install other dependencies
RUN pip install -r requirements.txt

# Install deepspeed and xtts-api-server
RUN pip install deepspeed xtts-api-server

# Explicitly install TTS
RUN pip install TTS

# Expose the container ports
EXPOSE 8020

# Run xtts_api_server when the container starts
CMD ["bash", "-c", "python3 -m xtts_api_server --listen -p 8020 -t 'http://localhost:8020' -sf 'xtts-server/speakers' -o 'xtts-server/output' -mf 'xtts-server/models' --deepspeed"]
