# PhysioVision Backend - Vision & Exercise Analysis

## Overview

Real-time computer vision module for analyzing and providing feedback on physiotherapy exercises. Uses OpenCV, MediaPipe, and TensorFlow models to track body posture and movement patterns.

## Supported Exercises

1. **Squats** - Lower body strength and form analysis
2. **Lunges** - Balance, alignment, and leg strength feedback
3. **Warrior Pose** - Yoga pose form and stability assessment
4. **Leg Raises** - Core strength and form analysis

## Prerequisites

- Python 3.11+
- Webcam or video input device
- 4GB+ RAM recommended
- NVIDIA GPU recommended (optional, for faster processing)

## Installation

### 1. Set up Python Virtual Environment

```bash
cd Backend_Vision
python -m venv venv

# On Windows:
venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

This includes:

- OpenCV for computer vision
- MediaPipe for pose estimation
- TensorFlow/Keras for custom models
- Various support libraries

## Running the Vision Backend

### Start WebSocket Server

```bash
python main.py
```

The server will start on `ws://localhost:8001` by default.

### Testing Individual Exercise Analysis

```bash
# Test Squats
python squats.py

# Test Lunges
python lunges_vision.py

# Test Warrior Pose
python WarriorPose.py

# Test Leg Raises
python legRaises.py
```

## Project Structure

```
Backend_Vision/
├── main.py                      # Main WebSocket server
├── squats.py                    # Squat exercise analyzer
├── lunges_vision.py             # Lunge exercise analyzer
├── WarriorPose.py              # Warrior pose analyzer
├── legRaises.py                # Leg raise analyzer
├── bark_tts.py                 # Text-to-speech feedback
├── test.py                      # Testing utilities
├── requirements.txt             # Python dependencies
├── models_vision/               # Trained ML models
│   ├── best_squat_model.keras
│   ├── preprocessed_data_scaler.joblib
│   └── preprocessed_data_label_encoder.joblib
└── README.md                   # This file
```

## Key Features

### Pose Detection & Analysis

- Real-time human pose estimation using MediaPipe
- 33-point body landmark detection
- Frame-by-frame analysis for movement tracking

### Custom Models

- Pre-trained Keras models for exercise classification
- Scikit-learn models for data preprocessing (scaling, encoding)
- Dynamic model loading and inference

### Real-time Feedback

- Form assessment with posture analysis
- Movement quality scoring
- Audio feedback via text-to-speech (edge_tts)
- Multi-language support (googletrans)

### WebSocket Communication

- Real-time data streaming to frontend
- Frame-by-frame analysis results
- Exercise progress updates
- Form correction suggestions

## Configuration

### Camera Settings

Modify camera index in exercise files:

```python
cap = cv2.VideoCapture(0)  # 0 = default camera, 1+ for other cameras
```

### Model Paths

Update model loading paths if using custom models:

```python
model = load_model('models_vision/your_model.keras')
```

### Frame Rate

Adjust frame processing rate for performance:

```python
FPS = 30  # Frames per second for analysis
```

## Development Guidelines

- Use MediaPipe landmarks (0-32) consistently across all analyzers
- Normalize pose data before model inference
- Implement confidence thresholding for landmark detection
- Log analysis results for debugging

## Performance Optimization

### GPU Acceleration

If you have CUDA-compatible NVIDIA GPU:

```bash
pip install tensorflow-gpu
```

Then modify in your files:

```python
import tensorflow as tf
gpus = tf.config.list_physical_devices('GPU')
```

### CPU Optimization

For CPU-only systems, reduce frame resolution:

```python
frame = cv2.resize(frame, (640, 480))  # Lower resolution for faster processing
```

## Troubleshooting

### Camera Not Detected

```bash
# List available cameras
python -c "import cv2; print(cv2.getBuildInformation())"
```

Try different camera indices (0, 1, 2, etc.):

```python
cap = cv2.VideoCapture(1)
```

### Model Loading Error

Ensure model files exist:

```bash
ls models_vision/
# Should show: best_squat_model.keras, preprocessed_data_scaler.joblib, etc.
```

### Low FPS/Slow Processing

- Reduce frame resolution
- Use GPU acceleration if available
- Reduce model complexity
- Close unnecessary applications

### WebSocket Connection Issues

Ensure the WebSocket server is running:

```bash
python main.py
```

Verify port is available:

```bash
netstat -an | grep :8001  # On Windows
netstat -an | grep :8001  # On Unix
```

## API Communication

### WebSocket Message Format

**From Frontend to Vision Server:**

```json
{
  "exercise": "squat",
  "action": "start"
}
```

**From Vision Server to Frontend:**

```json
{
  "exercise": "squat",
  "frame_num": 42,
  "confidence": 0.95,
  "feedback": "Good form, lower a bit more",
  "score": 85,
  "landmarks": [[x1, y1, z1], [x2, y2, z2], ...]
}
```

## License

Proprietary - PhysioVision Project
