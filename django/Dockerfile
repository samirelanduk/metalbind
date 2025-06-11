# Use the official Python slim image
FROM --platform=$TARGETPLATFORM python:3.12-slim-bookworm

# Set environment variables to prevent Python from writing .pyc files to disk
# and to buffer stdout and stderr
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1

# Create metalbind user and app directory
RUN useradd -m metalbind && mkdir -p /app && chown -R metalbind:metalbind /app

# Set the working directory
WORKDIR /app

# Install Python dependencies
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt && pip install gunicorn psycopg2-binary

# Copy the Django project
COPY --chown=metalbind:metalbind core /app/core
COPY --chown=metalbind:metalbind manage.py /app/manage.py

# Switch to the metalbind user
USER metalbind

# Expose port 8000 for the Django app
EXPOSE 8000

# Run the Gunicorn server
CMD ["gunicorn", "--workers", "2", "--bind", "0.0.0.0:8000", "core.wsgi:application"]