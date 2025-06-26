# Audio Emotion and Genre Classification System
## Key Features
### 🧠 Dual-Model Architecture
- **Emotion Recognition**: CNN model trained on RAVDESS dataset (8 emotions)
- **Genre Classification**: SVM model trained on GTZAN dataset (10 genres)

### ⚡ Real-Time Processing
- Live audio recording via Web Audio API
- <300ms inference latency
- Streaming audio processing

### 🛠️ Technical Highlights
- **Feature Extraction**: 40 MFCC coefficients using Librosa
- **Web Interface**: 
  - File upload and live recording
  - Real-time results display
  - Responsive design
- **Backend**: 
  - Flask REST API endpoints
  - Asynchronous processing
  - Error logging

## Performance Metrics
| Task        | Model   | Accuracy | Dataset     |
|-------------|---------|----------|-------------|
| Emotion     | CNN     | 89%      | RAVDESS     |
| Genre       | SVM     | 87%      | GTZAN       |

## Installation

### Prerequisites
- Python 3.8+
- FFmpeg (`sudo apt install ffmpeg`)

### Setup

Create virtual environment
python -m venv venv
source venv/bin/activate

Install dependencies
pip install -r requirements.txt

Run application
python app.py


