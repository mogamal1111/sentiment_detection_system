# People Sentiment Analysis System
Advanced real-time sentiment analysis system for comprehensive emotion monitoring.

## System Overview
This system uses sophisticated machine learning models for real-time sentiment analysis:
- **Sentiment Analysis**: Advanced emotion recognition and sentiment classification
- **Multi-Stream Support**: Monitor multiple areas simultaneously


## Manual Installation

### Prerequisites Setup
```bash
# Install system dependencies
sudo apt update
sudo apt install cmake python3-dev python3-pip python3-venv

# Setup MQTT broker (if not already installed)
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
sudo apt install mosquitto mosquitto-clients
sudo tee -a /etc/mosquitto/mosquitto.conf << EOF
listener 1883
protocol mqtt

listener 9001
protocol websockets

allow_anonymous true
EOF
sudo systemctl restart mosquitto
sudo systemctl enable mosquitto
```

### Application Setup
```bash
# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Upgrade pip and install requirements
pip install -U pip
pip install -r requirements.txt
```

### Configuration
```bash
# Interactive configuration (recommended)
python configure.py

# Or manually edit config.json
# Set local_video: true and local_video_source for local testing
# Configure RTSP streams for live camera feeds
```

### Running the Application
```bash
# Activate virtual environment
source venv/bin/activate

# Run with configuration file
python sentiment.py config.json
```

## Configuration Options

### Stream Configuration
- **Demo Mode**: Uses local video files for testing
- **Live Mode**: Connects to RTSP camera streams
- **Multi-Stream**: Monitor multiple areas simultaneously

### Detection Parameters
- **Inference Interval**: Time between detection runs (seconds)
- **Frame Dimensions**: Configurable input and output frame sizes
- **JPEG Quality**: Adjustable compression for streaming and storage

### Integration Settings
- **Data Send URL**: Endpoint for sentiment analysis reports
- **Heartbeat URL**: System health monitoring endpoint
- **MQTT Settings**: Real-time alert configuration

## Background Service Management

The project includes systemd service integration for production deployment:


```bash
# Install as system service
sudo cp sentiment-system.service /etc/systemd/system/hajj-systems.service
sudo systemctl daemon-reload
sudo systemctl enable hajj-systems.service
sudo systemctl start hajj-systems.service

sudo systemctl status hajj-systems.service
```


## Project Structure
```
sentiment-analysis_release/
├── sentiment.py             # Main application script
├── config.json             # Configuration file
├── requirements.txt        # Python dependencies
├── sentiment-system.service  # Systemd service file
├── models/                # RKNN model files
│   └── yolov11s-face_rknn_model
├── demo/                  # Demo videos and test files
├── libraries/             # Supporting libraries
│   ├── async_capture.py   # Async video capture
│   ├── datasend.py        # Data upload utilities
│   ├── stream_publisher.py # Stream publishing
│   └── utils.py           # Utility functions
└── uploader_cached_files/ # Temporary upload cache
```

## Model Information
- **Target Platform**: RK3588 (Rockchip Neural Processing Unit)
- **Format**: RKNN optimized models for edge inference
- **Performance**: Real-time processing with low latency

## API Integration
The system integrates with external APIs for comprehensive monitoring:
- **Sentiment Reporting**: Automated emotion analysis reporting
- **Health Monitoring**: System heartbeat and status
- **Authentication**: Secure API communication with secret keys
- **Live Streaming**: Real-time video feed publishing

## Troubleshooting

### Common Issues
1. **RKNN Model Loading**: Ensure models are in the correct format and path
2. **MQTT Connection**: Verify mosquitto service is running
3. **Camera Access**: Check RTSP URLs and network connectivity
4. **Permission Issues**: Ensure proper file permissions for service files
5. **Memory Usage**: Monitor system resources for optimal performance

### Debug Mode
Enable detailed logging by setting `logging_level: "debug"` in config.json

### Log Files
- **Application Logs**: Check console output or journal logs for services
- **System Logs**: Use `journalctl` for systemd service debugging
- **Stream Logs**: Monitor individual stream processing status

## Development
For development and testing:
```bash
# Test with demo video
python sentiment.py config.json

# Monitor in debug mode
# Edit config.json and set: "logging_level": "debug", "draw": true


# Check model performance
# Monitor CPU/GPU usage during inference
```

## Performance Optimization
- **Frame Rate**: Adjust inference_interval for optimal performance
- **Resolution**: Balance frame quality with processing speed
- **Batch Processing**: Configure multi-stream processing efficiently
- **Resource Management**: Monitor memory and CPU usage
