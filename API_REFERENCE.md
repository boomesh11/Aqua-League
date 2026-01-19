# Aqua-League SONAR INTEL - API Reference

## Overview
This document provides complete API reference for the SONAR INTEL AI-based Early Warning System for Oceanic Disasters.

## Core Modules

### 1. Detection Module

#### `detect_plankton(image: np.ndarray) -> dict`
Detects plankton species and populations in marine imagery.

**Parameters:**
- `image` (np.ndarray): Input image (H, W, 3) in RGB format

**Returns:**
```python
{
    "detections": [
        {
            "species": str,
            "confidence": float (0-1),
            "bbox": [x1, y1, x2, y2],
            "count": int
        }
    ],
    "processing_time_ms": float,
    "alert_status": bool
}
```

**Example:**
```python
from sonar_intel.detection import detect_plankton
import cv2

image = cv2.imread('marine_sample.jpg')
result = detect_plankton(image)
print(f"Alert: {result['alert_status']}")
```

### 2. Analysis Module

#### `analyze_water_quality(sensor_data: dict) -> dict`
Analyzes water quality metrics from sensor data.

**Parameters:**
- `sensor_data` (dict): Dictionary containing:
  - `temperature`: float (Celsius)
  - `salinity`: float (PSU)
  - `ph`: float
  - `dissolved_oxygen`: float (mg/L)

**Returns:**
```python
{
    "quality_score": float (0-100),
    "status": str ("GOOD"|"WARNING"|"CRITICAL"),
    "recommendations": [str],
    "risk_level": float
}
```

### 3. Alert System

#### `generate_alert(risk_level: float, alert_type: str) -> Alert`
Generates system alert for detected anomalies.

**Parameters:**
- `risk_level` (float): Risk score (0-1)
- `alert_type` (str): Type of alert ("BLOOM"|"TEMPERATURE"|"OXYGEN")

**Returns:**
```python
class Alert:
    timestamp: datetime
    severity: str ("LOW"|"MEDIUM"|"HIGH"|"CRITICAL")
    message: str
    coordinates: Tuple[float, float]
    recommended_action: str
```

## Configuration

### Environment Variables
```bash
SONAR_MODEL_PATH=/path/to/model.tflite
SONAR_LOG_LEVEL=INFO
SONAR_ALERT_THRESHOLD=0.7
SONAR_MQTT_BROKER=mqtt.example.com
```

## Error Handling

### Exception Types

#### `ModelLoadError`
Raised when model initialization fails.

#### `InferenceError`
Raised during model inference if unexpected conditions occur.

#### `ConfigurationError`
Raised when configuration is invalid or missing required parameters.

## Performance Metrics

- **Inference Latency**: ~45ms (Raspberry Pi 4)
- **Model Size**: ~8.2 MB (quantized)
- **Memory Usage**: ~120 MB
- **FPS (30fps input)**: Real-time processing

## Integration Examples

### MQTT Integration
```python
import paho.mqtt.client as mqtt
from sonar_intel import SonarIntel

sonar = SonarIntel()

def on_message(client, userdata, msg):
    sensor_data = json.loads(msg.payload)
    analysis = sonar.analyze(sensor_data)
    # Publish results
    client.publish('sonar/results', json.dumps(analysis))

client = mqtt.Client()
client.on_message = on_message
client.connect('mqtt.broker.com', 1883, 60)
client.subscribe('sonar/sensor_data')
client.loop_forever()
```

## Versioning
- Current Version: 1.0.0
- Python Support: 3.8+
- TensorFlow Version: 2.10+

## License
MIT License

---
**Last Updated:** January 2026
