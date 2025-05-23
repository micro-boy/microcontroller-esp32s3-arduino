# 🚢 Ship Detection System - BAIMBAI 2025

## Sistem Monitoring Kapal Tongkang Batu Bara - Jembatan Mahakam Samarinda

---

## 📋 Deskripsi Project

Sistem monitoring real-time untuk mencegah kapal tongkang batu bara menabrak tiang Jembatan Mahakam Samarinda menggunakan **NVIDIA Jetson Nano** dan **Computer Vision**.

### 🎯 Fitur Utama:
- ✅ **Real-time Ship Detection** - Deteksi kapal tongkang secara real-time
- ✅ **Virtual Safety Zones** - Zona aman dan bahaya virtual
- ✅ **Alert System** - Peringatan buzzer otomatis
- ✅ **Demo Mode** - Simulasi untuk presentasi
- ✅ **Performance Monitoring** - Monitoring FPS dan performa

---

## 🛠️ Hardware Requirements

### Minimum:
- **NVIDIA Jetson Nano Developer Kit**
- **CSI Camera Module** (Raspberry Pi Camera V2 compatible)
- **Buzzer** (opsional, untuk alert)
- **MicroSD Card** 32GB+
- **Power Supply** 5V/4A

### Yang Anda Miliki:
- ✅ Jetson Nano dengan JetPack 4.6.6
- ✅ Ubuntu 18.04
- ✅ Python 3.6.9
- ✅ CUDA 10.2.300
- ✅ CSI Camera terpasang

---

## 🚀 Quick Start Guide

### 1. Setup Environment
```bash
# Download semua file ke direktori project
mkdir ship_detection_system
cd ship_detection_system

# Copy semua script yang telah dibuat

# Jalankan setup
chmod +x 2_install_dependencies.sh
./2_install_dependencies.sh
```

### 2. Test System
```bash
# Test lengkap sistem
python3 5_setup_complete.py

# Test kamera CSI
python3 1_test_camera.py
```

### 3. Run Demo
```bash
# Demo simulation (tanpa kamera)
python3 6_run_demo.py --demo

# Live camera mode
python3 6_run_demo.py --live

# Auto-detect mode (default)
python3 6_run_demo.py
```

---

## 🎮 Controls Saat Demo

| Key | Function |
|-----|----------|
| `q` | Quit/Keluar |
| `s` | Screenshot |
| `d` | Toggle Demo/Live mode |
| `r` | Reset system |
| `n` | Next demo scenario |

---

## 📊 Demo Scenarios

### 1. **Safe Navigation** 
Kapal navigasi aman di zona tengah

### 2. **Left Danger**
Kapal mendekati tiang kiri (ALERT!)

### 3. **Right Danger** 
Kapal mendekati tiang kanan (ALERT!)

### 4. **Multiple Ships**
Multiple kapal dengan path berbeda

---

## 🎯 Cara Kerja Sistem

### Virtual Zones:
```
┌─────────────────────────────────────────┐
│ DANGER │    SAFE ZONE    │    DANGER   │
│ (LEFT) │    (CENTER)     │   (RIGHT)   │
│  🔴    │       🟢        │     🔴      │
└─────────────────────────────────────────┘
```

### Detection Logic:
1. **Input**: Camera CSI atau Demo simulation
2. **Processing**: OpenCV + Computer Vision
3. **Detection**: Ship identification & tracking
4. **Zone Analysis**: Check posisi kapal vs virtual zones
5. **Alert**: Buzzer + visual warning jika bahaya

---

## 🔧 Technical Details

### Dependencies:
- `OpenCV 4.1.1` - Computer vision
- `NumPy` - Array processing  
- `Python 3.6.9` - Main language
- `RPi.GPIO` - Buzzer control (optional)

### Performance:
- **Target FPS**: 15-30 FPS
- **Detection Range**: Sesuai field of view kamera
- **Alert Response**: < 1 second
- **Accuracy**: High precision untuk kapal tongkang

---

## 📁 File Structure

```
ship_detection_system/
├── 1_test_camera.py           # Test kamera CSI
├── 2_install_dependencies.sh  # Install dependencies  
├── 3_ship_detection_system.py # Main detection system
├── 4_demo_simulator.py        # Demo simulator
├── 5_setup_complete.py        # Setup & testing
├── 6_run_demo.py             # Main demo runner
├── README.md                 # Dokumentasi ini
└── screenshots/              # Screenshot results
```

---

## 🎭 Untuk Demo Presentasi

### Preparation Checklist:
- [ ] Test kamera: `python3 1_test_camera.py`
- [ ] Verify system: `python3 5_setup_complete.py`  
- [ ] Practice demo: `python3 6_run_demo.py --demo`
- [ ] Screenshot ready: Press `s` during demo
- [ ] Backup plan: Demo mode jika camera issue

### Demo Flow:
1. **Intro** - Explain problem & solution
2. **Show Virtual Zones** - Explain zone concept
3. **Safe Navigation** - Show normal operation
4. **Danger Scenarios** - Trigger alerts
5. **Live Camera** - Switch to real camera if available
6. **Q&A** - Answer questions

---

## 🚨 Troubleshooting

### Camera Issues:
```bash
# Check camera connection
ls /dev/video*

# Test dengan gstreamer
gst-launch-1.0 nvarguscamerasrc ! nvoverlaysink
```

### GPIO Issues:
- Normal jika tidak ada buzzer hardware
- System akan simulate buzzer output

### Performance Issues:
- Reduce camera resolution
- Lower detection sensitivity
- Check CPU/GPU usage dengan `htop`

---

## 📈 Future Enhancements

### Phase 2 (Post-Demo):
- [ ] **YOLO Integration** - Advanced ship detection
- [ ] **Distance Estimation** - Calculate real distance
- [ ] **Network Alerts** - Send alerts via network
- [ ] **Data Logging** - Log all detections
- [ ] **Mobile App** - Remote monitoring

### Phase 3 (Production):
- [ ] **Weather Resistance** - Weatherproof housing
- [ ] **Night Vision** - IR camera support
- [ ] **Multi-Camera** - Multiple monitoring points
- [ ] **AI Training** - Custom model untuk Mahakam river

---

## 👥 Team & Support

**Developer**: Tim BAIMBAI 2025  
**Location**: Samarinda, East Kalimantan, Indonesia  
**Target**: Jembatan Mahakam Safety System

### Contact:
- Demo questions: Ask during presentation
- Technical issues: Check troubleshooting section
- Future development: Post-competition discussion

---

## 🏆 Competition Details

**Event**: BAIMBAI 2025  
**Category**: Smart City / IoT Solution  
**Problem**: Kapal tongkang menabrak tiang jembatan  
**Solution**: AI-powered early warning system  
**Timeline**: 1 week development + demo  

---

## 📄 License & Credits

- **OpenCV**: BSD License
- **NVIDIA JetPack**: NVIDIA License  
- **Python Libraries**: Various open source licenses
- **Project Code**: Developed for BAIMBAI 2025

---

## 🎉 Demo Ready!

**Your system is ready for presentation!** 

Run `python3 6_run_demo.py` dan mulai demo Anda.

**Good luck with BAIMBAI 2025!** 🚀🏆
