# Dockerfile

# Use the official Debian base image
FROM debian:buster-slim

# Install Python, curl, ping, and other dependencies
RUN apt-get update && \
    apt-get install -y python3 python3-pip nginx curl iputils-ping && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Set the working directory
WORKDIR /app

# Copy the application files
COPY app.py /app/
COPY requirements.txt /app/

# Install the Python dependencies
RUN pip3 install --no-cache-dir -r requirements.txt

# Remove the default Nginx configuration
RUN rm /etc/nginx/sites-enabled/default

# Copy custom Nginx configuration
COPY nginx.conf /etc/nginx/sites-available/
RUN ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled

# Expose the port
EXPOSE 80

# Run Gunicorn and Nginx
CMD service nginx start && gunicorn -w 4 -b 0.0.0.0:8000 app:app
