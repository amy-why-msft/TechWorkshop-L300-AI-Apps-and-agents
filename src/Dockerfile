
FROM python:3.12-slim

# Upgrade pip and system packages to reduce vulnerabilities
RUN apt-get update && apt-get upgrade -y && pip install --upgrade pip && rm -rf /var/lib/apt/lists/*

# Set work directory
WORKDIR /app

# Install system dependencies (including ODBC for pyodbc, OpenCV, and all common video/image libraries)
RUN apt-get update && apt-get install -y \
    build-essential unixodbc-dev gcc g++ \
    libgl1 libglib2.0-0 libsm6 libxext6 libxrender1 \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt


# Copy the rest of the code
COPY . .

# Copy .env file into the image (for python-dotenv)
# When building with BuildKit, the .env file will be created from a secret
# For local builds, you can use: COPY .env .env
# For GitHub Actions builds with secrets, the .env file will be created via RUN --mount=type=secret
RUN --mount=type=secret,id=env_content set -e; \
    if [ -f /run/secrets/env_content ]; then \
        cat /run/secrets/env_content > .env; \
        echo "Successfully created .env file from secret"; \
    else \
        echo "ERROR: No secret mounted. .env file not created."; \
        echo "For production builds, pass --secret id=env_content,src=.env"; \
        exit 1; \
    fi

# Expose the port (default FastAPI/uvicorn port)
EXPOSE 8000

# Set environment variables (override in production)
ENV PORT=8000

# Start the FastAPI app
CMD ["uvicorn", "chat_app:app", "--host", "0.0.0.0", "--port", "8000"]
