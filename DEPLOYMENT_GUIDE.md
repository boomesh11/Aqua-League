# Aqua-League SONAR INTEL - Deployment Guide

## Overview
This guide provides step-by-step instructions for deploying the SONAR INTEL AI-based Early Warning System for oceanic disasters on edge devices including Raspberry Pi and cloud platforms.

## Prerequisites
- Python 3.8+
- TensorFlow Lite runtime
- Raspberry Pi 4 or higher (for edge deployment)
- 4GB+ RAM
- Internet connectivity for initial setup

## Local Development Setup

### 1. Clone the Repository
```bash
git clone https://github.com/boomesh11/Aqua-League.git
cd Aqua-League
```

### 2. Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

## Raspberry Pi Deployment

### Prerequisites for Raspberry Pi
- Raspberry Pi OS (64-bit recommended)
- SSH access enabled

### Installation Steps

1. **Update System**
```bash
sudo apt-get update && sudo apt-get upgrade -y
```

2. **Install Python and Dependencies**
```bash
sudo apt-get install python3-dev python3-pip python3-venv -y
sudo apt-get install libatlas-base-dev libjasper-dev libtiff5 libjasper1 libharfbuzz0b libwebp6 libtiff5 -y
```

3. **Install TensorFlow Lite
```bash
pip install tensorflow-lite
```

4. **Deploy Application**
```bash
git clone https://github.com/boomesh11/Aqua-League.git
cd Aqua-League
pip install -r requirements-pi.txt
```

## Running SONAR INTEL

### Start the Service
```bash
python sonar_intel.py --mode production
```

### Configuration
Edit `config.yaml` to customize:
- Detection sensitivity
- Alert thresholds
- Data logging intervals
- Cloud sync settings

## Monitoring

Check system status:
```bash
ps aux | grep sonar_intel
```

View logs:
```bash
tail -f logs/sonar_intel.log
```

## Troubleshooting

### Issue: GPU Memory Error
**Solution:** Reduce model precision or use INT8 quantization

### Issue: Slow Inference
**Solution:** Enable TensorFlow Lite GPU acceleration or use edge TPU

### Issue: Network Connectivity
**Solution:** Check WiFi/Ethernet configuration and firewall rules

## Performance Optimization

### Model Quantization
```python
from tensorflow.lite.python import lite

converter = lite.TFLiteConverter.from_concrete_functions([concrete_func])
converter.optimizations = [lite.Optimize.DEFAULT]
quantized_model = converter.convert()
```

### Inference Benchmark
```bash
python benchmark.py --model model.tflite --input_size 224,224
```

## Production Deployment Checklist

- [ ] Model quantized and optimized
- [ ] All dependencies installed
- [ ] Configuration validated
- [ ] Logs configured
- [ ] Monitoring setup complete
- [ ] Backup system in place
- [ ] Security hardening applied

## Support

For issues or questions, please create an issue on GitHub or contact the development team.

---
**Last Updated:** January 2026
**Version:** 1.0
