FROM python:3.11-slim

# Install Tesseract OCR + poppler (for PDF support) as system packages
RUN apt-get update && apt-get install -y \
    tesseract-ocr \
    tesseract-ocr-eng \
    poppler-utils \
    libglib2.0-0 \
    libsm6 \
    libxrender1 \
    libxext6 \
    libgl1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app files
COPY . .

# Create local fallback folders (volume at /app/data is mounted at runtime)
RUN mkdir -p uploads generated_xls

EXPOSE 8080

# DB is initialised automatically when app.py is imported (see bottom of app.py)
CMD ["gunicorn", "--bind", "0.0.0.0:8080", "--workers", "2", "--timeout", "120", "app:app"]
