# Dockerfile for Challenge 1a


# FROM --platform=linux/amd64 python:3.10-slim
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy script into container

COPY . .

# Install dependencies if any (example shown)
RUN pip install -r requirements.txt

# When the container runs, execute the script with default input/output folders
CMD ["python", "main.py", "/app/input", "/app/output"]

