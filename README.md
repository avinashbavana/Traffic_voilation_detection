# 🏍️ Two-Wheeler Traffic Violation Detection System

An AI-powered web application for detecting traffic violations in two-wheeler vehicles using computer vision, multiple Roboflow models, and OCR technology. Optimized for Railway deployment with Tesseract-only OCR.

![Python](https://img.shields.io/badge/Python-3.10.13-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-2.3.3-000000?style=for-the-badge&logo=flask&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-4.8.1-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Gunicorn](https://img.shields.io/badge/Gunicorn-21.2.0-499848?style=for-the-badge&logo=gunicorn&logoColor=white)
![Render](https://img.shields.io/badge/Render-Deployed-46E3B7?style=for-the-badge&logo=render&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Live-success?style=for-the-badge)

## 📋 Table of Contents

- [Features](#features)
- [Live Demo](#live-demo)
- [Detection Capabilities](#detection-capabilities)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Usage](#usage)
- [API Models](#api-models)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

### Core Detection Capabilities
- **🪖 No Helmet Detection**: Identifies riders without helmets
- **👥 Triple Riding Detection**: Detects more than 2 passengers on a two-wheeler
- **🚦 Red Light Violation**: Identifies vehicles crossing during red signals
- **📄 License Plate Recognition**: Automated number plate extraction using Tesseract OCR

### Smart Features
- **⏱️ Violation Cooldown System**: 5-second cooldown to prevent duplicate detections
- **🎯 Multi-Model AI**: Uses 4 specialized Roboflow models simultaneously
- **📸 Evidence Collection**: Automatically saves violation images with annotations
- **💾 SQLite Database**: Persistent storage of all violations
- **📊 Analytics Dashboard**: Real-time violation statistics and summaries
- **🎥 Video & Image Support**: Process both videos and images

### User Interface
- **Modern Web Interface**: Clean, responsive Bootstrap 5 design
- **Real-time Processing**: Live updates during video analysis
- **Violation Gallery**: Browse all captured violations
- **Downloadable Results**: Download processed videos and evidence
- **Mobile-Friendly**: Works on all devices

## 🎬 Live Demo

### Supported File Formats
- **Videos**: MP4, AVI, MOV, MKV, WEBM
- **Images**: JPG, JPEG, PNG
- **Max Size**: 500MB

## 🎯 Detection Capabilities

### 1. No Helmet Detection
- Identifies riders without helmets
- Uses specialized helmet detection model
- Confidence threshold: 0.3

### 2. Triple Riding Detection
- Detects more than 2 passengers on two-wheeler
- Class: "people" (more than 2 passengers)
- Confidence threshold: 0.3

### 3. Red Light Violation
- Detects vehicles crossing during red signals
- Traffic light model with class "1" for red light
- Combines with bike detection for accurate violation

### 4. License Plate Recognition
- Haar Cascade detection for plate localization
- Tesseract OCR for text extraction
- Multiple enhancement techniques for accuracy
- Automatic plate formatting

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Web Browser                       │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│              Flask Web Server                       │
│              (Port: Railway/3000)                   │
└──────────────────┬──────────────────────────────────┘
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
┌──────────────┐      ┌──────────────┐
│   SQLite     │      │  Processing  │
│   Database   │      │    Engine    │
└──────────────┘      └──────┬───────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
    ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
    │  Roboflow   │  │   License   │  │  Evidence   │
    │   Models    │  │    Plate    │  │   Storage   │
    │  (4 Models) │  │  Detection  │  │             │
    └─────────────┘  └─────────────┘  └─────────────┘
```

### Roboflow Model Pipeline

```
Frame Input
    │
    ├──► Two-Wheeler Detection (two-wheeler-aj3hv/2)
    │
    ├──► Rider/Helmet Detection (two-person-ride/11)
    │
    ├──► Triple Riding Detection (more-than-two-passengers/1)
    │
    └──► Traffic Light Detection (traffic-lights-ydbwt/1)
         │
         ▼
    Violation Analysis
         │
         ├──► License Plate Extraction
         │
         ├──► Tesseract OCR
         │
         └──► Evidence Generation
              │
              ▼
         Save to Database
```

## 🚀 Installation

### Prerequisites

- **Python**: 3.10.13
- **Tesseract OCR**: 4.0+ (required)
- **Roboflow API Key**: Required for model inference
- **Operating System**: Windows, Linux, or macOS

### Step 1: Clone the Repository

```bash
git clone https://github.com/kumarvishal01971/Traffic_violation_detection.git
cd Traffic_violation_detection
```

### Step 2: Create Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/macOS
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Install Tesseract OCR

**Windows:**
1. Download installer: https://github.com/UB-Mannheim/tesseract/wiki
2. Install to: `C:\Program Files\Tesseract-OCR\`
3. Add to system PATH
4. Update path in `app.py` if needed:
```python
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get update
sudo apt-get install tesseract-ocr
```

**macOS:**
```bash
brew install tesseract
```

### Step 5: Get Roboflow API Key

1. Sign up at [Roboflow](https://roboflow.com/)
2. Get your API key from workspace settings
3. Update in `app.py`:
```python
def __init__(self, api_key: str = "YOUR_API_KEY_HERE"):
```

### Step 6: Create Required Directories

```bash
mkdir uploads outputs violation_evidence
```

### Step 7: Add Haar Cascade File (Optional)

Download `haarcascade_russian_plate_number.xml` and place in root directory for better plate detection.

## 📦 Requirements

```txt
flask==2.3.3
werkzeug==2.3.7
opencv-python-headless==4.8.1.78
numpy==1.26.4
pillow==10.1.0
inference-sdk==0.9.0
gunicorn==21.2.0
```

### Why These Versions?

- **opencv-python-headless**: Lightweight version without GUI (Railway compatible)
- **inference-sdk**: Roboflow API client for model inference
- **gunicorn**: Production-grade WSGI server for deployment
- **No PyTorch/EasyOCR**: Keeps deployment lightweight and fast

## 🎯 Usage

### Local Development

```bash
python web_app.py
```

Access at: `http://localhost:3000`

### Using the Application

#### 1. Upload & Process

1. Go to **Upload** page
2. Select video (MP4, AVI, MOV) or image (JPG, PNG)
3. Click **Upload**
4. Processing starts automatically
5. View results when complete

#### 2. Run Demo

1. Add `demo_video.mp4` to `uploads/` folder
2. Click **Run Demo** on homepage
3. View processed results

#### 3. View Violations

1. Click **View Violations** on homepage
2. Browse all detected violations
3. Click on evidence images to enlarge
4. Filter by violation type

### Command Line Usage

```bash
# Process video
python app.py --video path/to/video.mp4 --out output.mp4

# Process image
python app.py --image path/to/image.jpg --out output.jpg
```

## 🤖 API Models

### Model Configuration

```python
self.models = {
    'two_wheeler': 'two-wheeler-aj3hv/2',
    'riders': 'two-person-ride/11',
    'triple_riding': 'more-than-two-passengers/1',
    'traffic_light': 'traffic-lights-ydbwt/1'
}
```

### Model Details

| Model | Purpose | Classes | Confidence |
|-------|---------|---------|------------|
| **two-wheeler-aj3hv/2** | Bike detection | bike, motorcycle | 0.5 |
| **two-person-ride/11** | Helmet detection | helmet, no_helmet | 0.5 |
| **more-than-two-passengers/1** | Triple riding | people | 0.5 |
| **traffic-lights-ydbwt/1** | Traffic lights | 0 (green), 1 (red) | 0.5 |

### Detection Logic

```python
# NO HELMET: "no_helmet" class detected
# TRIPLE RIDING: "people" class (>2 passengers)
# RED LIGHT: class "1" (red) + bike detected
```

## ⚙️ Configuration

### Adjust Processing Speed

```python
# In process_video() method
frame_skip = 3  # Process every 3rd frame
                # Higher = faster, may miss violations
                # Lower = slower, more accurate
```

### Change Confidence Thresholds

```python
self.confidence_thresholds = {
    'bike': 0.3,        # Bike detection
    'no_helmet': 0.3,   # No helmet detection
    'triple': 0.3,      # Triple riding
    'light': 0.3        # Traffic light
}
```

### Violation Cooldown

```python
# In __init__()
self.tracker = SimpleViolationTracker(cooldown_seconds=5.0)
# Prevents duplicate violations within 5 seconds
```

### Upload Limits

```python
# In web_app.py
app.config['MAX_CONTENT_LENGTH'] = 500 * 1024 * 1024  # 500MB
```

## Deployment
### Render Deployment (Recommended - Currently Live ✅)
1. Create runtime.txt:
txtpython-3.10.13
2. Configure Render Service:
Service Settings:

Runtime: Python 3
Build Command:

bash  pip install -r requirements.txt

Start Command:

bash  gunicorn flask_app:app
3. Set Environment Variables:
In Render Dashboard → Environment, add:
bashPYTHON_VERSION=3.10.13
ROBOFLOW_API_KEY=your_roboflow_api_key
PYTHONUNBUFFERED=1
4. Deploy:
bash# Push to GitHub
git add .
git commit -m "Deploy to Render"
git push origin main

# Render will automatically deploy from your connected GitHub repo
```

**Important Notes:**
- ✅ Remove any `Dockerfile`, `Procfile`, `nixpacks.toml`, or `railway.toml` files
- ✅ Render automatically assigns PORT (usually 10000)
- ✅ Free tier includes: 750 hours/month, automatic SSL, custom domains

---

### Heroku Deployment (Alternative)

#### 1. **Create `Procfile`:**
```
web: gunicorn flask_app:app --timeout 120
2. Create runtime.txt:
txtpython-3.10.13
3. Add Buildpack (if using Tesseract):
bashheroku buildpacks:add --index 1 heroku/python
4. Set Environment Variables:
bashheroku config:set ROBOFLOW_API_KEY=your_api_key
heroku config:set PYTHONUNBUFFERED=1
5. Deploy:
bashheroku login
heroku create your-app-name
git push heroku main

Docker Deployment
Dockerfile:
dockerfileFROM python:3.10-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender-dev \
    libgomp1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY . .

# Set environment variables
ENV PORT=10000
ENV PYTHONUNBUFFERED=1

EXPOSE 10000

# Run with gunicorn
CMD ["gunicorn", "flask_app:app", "--bind", "0.0.0.0:10000", "--workers", "2", "--timeout", "120"]
Build and Run:
bash# Build the image
docker build -t traffic-violation-detection .

# Run the container
docker run -p 10000:10000 \
  -e ROBOFLOW_API_KEY=your_api_key \
  traffic-violation-detection
Docker Compose (Optional):
yamlversion: '3.8'

services:
  web:
    build: .
    ports:
      - "10000:10000"
    environment:
      - ROBOFLOW_API_KEY=${ROBOFLOW_API_KEY}
      - PYTHONUNBUFFERED=1
    volumes:
      - ./uploads:/app/uploads
      - ./outputs:/app/outputs
      - ./violation_evidence:/app/violation_evidence
Run with Docker Compose:
bashdocker-compose up --build

## 📁 Project Structure

```
traffic-violation-detection/
│
├── app.py                              # Core detection engine
├── flask_app.py                          # Flask web application
├── requirements.txt                    # Python dependencies
├── README.md                           # This file
│
├── templates/                          # HTML templates
│   ├── base.html                       # Base template
│   ├── index.html                      # Homepage
│   ├── upload.html                     # Upload page
│   ├── result.html                     # Results page
│   ├── violations.html                 # Violations gallery
│   └── demo.html                       # Demo page
│
├── uploads/                            # Uploaded files
├── outputs/                            # Processed videos
├── violation_evidence/                 # Evidence images
│
├── violations.db                       # SQLite database
│
├── railway.toml                        # Railway config (deployment)
├── nixpacks.toml                       # Nixpacks config (deployment)
└── Procfile                            # Heroku config (deployment)
```

## 🛠️ Technologies Used

### Backend
- **Flask 2.3.3** - Web framework
- **OpenCV 4.8.1** - Computer vision (headless for deployment)
- **NumPy 1.26.4** - Numerical computations
- **Pillow 10.1.0** - Image processing
- **Gunicorn 21.2.0** - WSGI HTTP server

### AI/ML
- **Roboflow Inference SDK 0.9.0** - Model inference
- **Tesseract OCR** - License plate text extraction
- **4 Custom Models** - Specialized detection models

### Frontend
- **Bootstrap 5** - UI framework
- **JavaScript** - Interactive features
- **HTML5/CSS3** - Markup and styling

### Database
- **SQLite3** - Lightweight embedded database
- **No external DB required** - Portable and simple

### Deployment
- **Railway** - Primary deployment platform
- **Gunicorn** - Production WSGI server
- **Nixpacks** - Smart build system

## 🐛 Troubleshooting

### Common Issues

#### 1. "Tesseract not found"

**Solution:**
```bash
# Install Tesseract
# Ubuntu/Debian
sudo apt-get install tesseract-ocr

# Windows - download installer and add to PATH
# macOS
brew install tesseract

# Update path in app.py if needed
pytesseract.pytesseract.tesseract_cmd = r'/usr/bin/tesseract'
```

#### 2. "No module named 'inference_sdk'"

**Solution:**
```bash
pip install inference-sdk==0.9.0
```

#### 3. API Connection Error

**Possible causes:**
- Invalid API key
- No internet connection
- Roboflow service down

**Solution:**
```python
# Check API key in app.py
api_key = "YOUR_VALID_API_KEY"

# Test connection
from inference_sdk import InferenceHTTPClient
client = InferenceHTTPClient(
    api_url="https://serverless.roboflow.com",
    api_key="YOUR_API_KEY"
)
```

#### 4. No Violations Detected

**Possible causes:**
- Low confidence threshold
- Video quality issues
- Wrong model IDs

**Solutions:**
```python
# Lower confidence thresholds
self.confidence_thresholds = {
    'bike': 0.2,      # Try 0.2 instead of 0.3
    'no_helmet': 0.2,
    'triple': 0.2,
    'light': 0.2
}

# Check model predictions in console
print(f"Predictions: {all_predictions}")
```

#### 5. "Database locked" Error

**Solution:**
```bash
# Close all other connections to violations.db
# Or delete and recreate database
rm violations.db
python web_app.py  # Will recreate database
```

#### 6. Slow Processing

**Solutions:**
```python
# Increase frame skip
frame_skip = 5  # Instead of 3

# Reduce video resolution before upload
# Use smaller videos for testing
```

#### 7. Railway Deployment Fails

**Common fixes:**
```bash
# Ensure nixpacks.toml exists with tesseract
# Check logs: railway logs
# Verify environment variables are set
# Check buildpack order
```

## 📊 Database Schema

```sql
CREATE TABLE violations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT,
    plate_number TEXT,
    violation_type TEXT,
    confidence REAL,
    details TEXT,
    evidence_file TEXT,
    video_source TEXT
)
```

## 🎨 Customization

### Add New Violation Type

1. **Add model to `app.py`:**
```python
self.models = {
    # ... existing models
    'new_violation': 'your-model-id/version'
}
```

2. **Update detection logic:**
```python
def analyze_frame_simple(self, predictions, frame, frame_num):
    # Add new detection logic
    new_detections = [p for p in predictions if ...]
    
    if new_detections:
        violation = {
            'type': 'NEW_VIOLATION_TYPE',
            # ... rest of logic
        }
```

3. **Update UI in templates:**
```html
<!-- Add new violation type display -->
```

### Change Color Scheme

Edit violation colors in `app.py`:

```python
def save_evidence(self, frame, violation):
    if v_type == 'NO_HELMET':
        color = (0, 0, 255)      # Red (BGR)
    elif v_type == 'TRIPLE_RIDING':
        color = (255, 0, 255)    # Magenta
    # Add your custom colors
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guide
- Add docstrings to functions
- Test with sample videos before committing
- Update README if adding features

## 📝 License

This project is licensed under the MIT License.

```
MIT License

Copyright (c) 2024 Two-Wheeler Traffic Violation Detection

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```



