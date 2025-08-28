# Base image
FROM python:3.9-slim

# Maintainer + version info
LABEL maintainer="omar.kaabi@esprit.tn" \
      version="1.0"

# Set working directory
WORKDIR /app

# Copy dependencies and install them
COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py /app/app.py

# Expose port
EXPOSE 9090

# Command to run the app
CMD ["python", "app.py"]
