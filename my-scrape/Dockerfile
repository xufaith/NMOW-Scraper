FROM python:3.10-slim

# Install necessary system dependencies
RUN apt-get update && apt-get install -y \
    wget unzip curl xvfb fonts-liberation libasound2 libatk-bridge2.0-0 \
    libatk1.0-0 libcups2 libdbus-1-3 libgdk-pixbuf2.0-0 \
    libnspr4 libnss3 libx11-xcb1 libxcomposite1 libxdamage1 \
    libxrandr2 xdg-utils libgbm1 libgtk-3-0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Download Windows 64-bit Chrome
RUN wget -O chrome-win64.zip https://storage.googleapis.com/chrome-for-testing-public/135.0.7049.42/win64/chrome-win64.zip && \
    unzip chrome-win64.zip -d /opt/chrome && \
    rm chrome-win64.zip

# Download Windows 64-bit ChromeDriver
RUN wget -O chromedriver-win64.zip https://storage.googleapis.com/chrome-for-testing-public/135.0.7049.42/win64/chromedriver-win64.zip && \
    unzip chromedriver-win64.zip -d /opt/chromedriver && \
    rm chromedriver-win64.zip

# Add Chrome and ChromeDriver to PATH
ENV PATH="/opt/chrome/chrome-win64:${PATH}"
ENV PATH="/opt/chromedriver/chromedriver-win64:${PATH}"

# Copy requirements and install
COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

# Copy the application code
COPY . .

# Use gunicorn for production if using Flask
CMD ["gunicorn", "--bind", "0.0.0.0:8080", "main:app"]
