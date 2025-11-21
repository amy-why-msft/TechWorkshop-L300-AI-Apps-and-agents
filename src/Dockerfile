
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
# Priority: 1) BuildKit secret (for CI/CD), 2) Local .env file (for development)
# For GitHub Actions builds: secret is mounted via --secret id=env_content
# For local builds: .env file should exist in the build context
RUN --mount=type=secret,id=env_content set -e; \
    if [ -f /run/secrets/env_content ]; then \
        cat /run/secrets/env_content > .env; \
        echo "Successfully created .env file from BuildKit secret"; \
    elif [ -f .env ]; then \
        echo "Using existing .env file from build context (local development)"; \
    else \
        echo "ERROR: No .env configuration found!"; \
        echo "For CI/CD: pass --secret id=env_content,src=<file>"; \
        echo "For local: create .env file based on env_sample.txt"; \
        exit 1; \
    fi

# Expose the port (default FastAPI/uvicorn port)
EXPOSE 8000

# Set environment variables (override in production)
ENV PORT=8000

# Start the FastAPI app
CMD ["uvicorn", "chat_app:app", "--host", "0.0.0.0", "--port", "8000"]
