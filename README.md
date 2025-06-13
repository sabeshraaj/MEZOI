# Mezoi

A voice-controlled healthcare dashboard for real-time patient monitoring and data visualization.

## Overview

Mezoi is a healthcare monitoring system that provides real-time visualization of patient vital signs with voice command navigation. The system displays pulse pressure, temperature, SpO2, and respiratory rate data through interactive charts and summary statistics.

## Features

- **Real-time Data Visualization**: Interactive charts for patient vital signs
- **Voice Command Navigation**: Hands-free navigation using wake words
- **Live Data Updates**: WebSocket integration for real-time data streaming
- **Multi-metric Dashboard**: Monitor pulse pressure, temperature, SpO2, and respiratory rate
- **Responsive Design**: Mobile and desktop compatible interface

## Voice Commands

The system supports three voice commands for navigation:

| Voice Command | Action | Page |
|---------------|--------|------|
| "hey apollo" | Opens main dashboard | index.html |
| "patient progress" | Opens patient progress page | patient_progress.html |
| "show videos" | Opens demonstration videos | demonstration.html |

## Prerequisites

- Python 3.7+
- MongoDB
- Node.js (for frontend dependencies)
- Microphone access for voice commands

## Installation

### 1. Clone the Repository
```bash
git clone <repository-url>
cd mezoi
```

### 2. Install Python Dependencies
```bash
pip install flask flask-cors pymongo websockets pvporcupine pyaudio
```

### 3. Set up MongoDB
```bash
# Start MongoDB service
sudo systemctl start mongod

# Create database and collection
mongo
use school
db.createCollection("medtech")
```

### 4. Configure Porcupine Wake Words

#### Create Porcupine Access Keys
1. Visit [Picovoice Console](https://console.picovoice.ai/)
2. Sign up/Login to get your access key
3. Create custom wake word models for:
   - "hey apollo"
   - "patient progress" 
   - "show videos"

#### Set up Wake Word Files
1. Create a `wakewords` folder in the project root
2. Train and download `.ppn` files for each wake word
3. Update the file paths in the Python server code:

```python
configs = [
    ('index.html', 'hey apollo', '/path/to/wakewords/hey-apollo.ppn', 'YOUR_ACCESS_KEY'),
    ('patient_progress.html', 'patient progress', '/path/to/wakewords/patient-progress.ppn', 'YOUR_ACCESS_KEY'),
    ('demonstration.html', 'show videos', '/path/to/wakewords/show-videos.ppn', 'YOUR_ACCESS_KEY')
]
```

#### Videos
Create a folder named videos and add the video which you want to display in the demonstration page and then link them accordingly 

### 5. Project Structure
```
mezoi/
├── index.html                 # Main dashboard
├── patient_progress.html      # Patient progress page
├── demonstration.html         # Video demonstrations
├── style.css                  # Dashboard styles
├── script.js                  # Frontend logic
├── server.py                  # Backend server
├── wakewords/                 # Wake word model files
│   ├── hey-apollo.ppn
│   ├── patient-progress.ppn
│   └── show-videos.ppn
└── README.md
```

## Usage

### 1. Start the Server
```bash
python server.py
```

The server will start on `http://localhost:5000` with WebSocket support on port 8080.

### 2. Access the Dashboard
Open your browser and navigate to `http://localhost:5000`

### 3. Voice Commands
- Ensure microphone permissions are granted
- Speak clearly: "hey apollo", "patient progress", or "show videos"
- The system will automatically navigate to the corresponding page

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Serves main dashboard |
| `/api/patient-data` | GET | Returns patient vital signs data |
| `/get-current-page` | GET | Returns current active page |
| `/<filename>` | GET | Serves static files |

## Data Format

The system expects MongoDB documents with the following structure:
```json
{
  "pulse_pressure": 45,
  "temperature": 98.6,
  "spo2": 98,
  "respiratory_rate": 16,
  "timestamp": "2025-06-13T10:30:00"
}
```

## Screenshots

### index.html
![Image 1](https://github.com/sabeshraaj/mezoi/blob/main/images/image1.jpeg?raw=true)

### patient_progress.html
![Image 2](https://github.com/sabeshraaj/mezoi/blob/main/images/image2.jpeg?raw=true)

### demonstration.html
![Image 3](https://github.com/sabeshraaj/mezoi/blob/main/images/image3.jpeg?raw=true)

### wakewords file 
![Image 4](https://github.com/sabeshraaj/mezoi/blob/main/images/image4.jpeg?raw=true)

### videos file 
![Image 5](https://github.com/sabeshraaj/mezoi/blob/main/images/image5.jpeg?raw=true)


## Configuration

### Database Settings
- **Host**: your local mongodb host
- **Database**: your own database
- **Collection**: your own collection

### WebSocket Settings
- **Port**: 8080
- **Auto-reconnection**: Enabled

### Voice Recognition Settings
- **Sensitivity**: 0.65 (adjustable)
- **Sample Rate**: 16kHz
- **Audio Format**: 16-bit PCM

## Troubleshooting

### Common Issues

**Voice commands not working:**
- Check microphone permissions
- Verify Porcupine access keys are valid
- Ensure `.ppn` files are in the correct path

**Database connection errors:**
- Verify MongoDB is running: `sudo systemctl status mongod`
- Check database name and collection exist

**Chart not loading:**
- Ensure data exists in MongoDB collection
- Check browser console for JavaScript errors
- Verify API endpoint returns valid data

### Audio Issues
```bash
# Install audio dependencies (Ubuntu/Debian)
sudo apt-get install portaudio19-dev python3-pyaudio

# For macOS
brew install portaudio
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test voice commands and data visualization
5. Submit a pull request


## Support

For technical support or questions about implementation, please refer to the documentation or create an issue in the repository.
