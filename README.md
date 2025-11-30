# Fishpond Health Detector (Edge Impulse + Android)

This project is an Android app that uses an Edge Impulse machine learning model (“Transfer Learning (Images)” block) to detect **normal** vs **problem** conditions in fishponds from camera images and alos image upload.

- **Platform:** Android (Flutter)
- **ML platform:** Edge Impulse
- **Model type:** Image classification (normal vs problem fishpond)

---

## 1. Problem Statement

Fish farmers often struggle to quickly tell if a pond is healthy or if there is a problem (dirty water, algal bloom, low oxygen indicators, etc.).  
This app uses on-device ML to classify fishpond images into:

- `normal`
- `problem`

The goal is to give farmers and field workers a simple, offline-friendly tool to check pond health using a mobile phone.

---

## 2. Tech Stack

- **Mobile app:** Android (Flutter)
- **ML pipeline:** Edge Impulse
- **Model format:** TFLite from Edge Impulse `.eim`
- **Language:** Dart (for Flutter)
- **Target device:** Android phone with camera and image upload.

---

## 3. Dataset Preparation

### 3.1 Classes

The dataset has two main classes:

- `normal` – healthy / normal fishpond conditions
- `problem` – unhealthy / problematic conditions (e.g. dirty water, heavy foam, algal bloom, visible stress)

### 3.2 Data Collection

- **Source:** real fishpond images collected from local fishpond farmers.
- **Total images:**
  - Normal: **750** images
  - Problem: **600** images
- **Image type:** close-up pond images (top view / side view), resized and normalized by Edge Impulse.
- **Preprocessing:**
  - Uploaded to Edge Impulse via Data acquisition
  - Labeled manually as `normal` or `problem`
  - Split into **training** and **testing** sets using Edge Impulse split tool (default 80/20 or your exact split).

---

## 4. Edge Impulse Workflow

This project strictly follows the Edge Impulse workflow.

### 4.1 Project

- **Edge Impulse project name:** `Fishpond`
- **Public project link:**  
  `https://studio.edgeimpulse.com/studio/839830` 

### 4.2 Impulse Design

Main blocks used:

1. **Input:**  
   - Image (RGB), size: `96 x 96`

2. **Processing block:**  
   - Image

3. **Learning block:**
   - Transfer Learning (Images) with 2 classes (normal, problem)

### 4.3 Model Training

- Number of training cycles: **20**
- Learning rate: **0.0005**

Training results:
- **Training accuracy:** ~**92.6%**
- **F1-score (per class):**
  - Normal: **0.93**
  - Problem: **0.92**

### 4.4 Deployment

Deployed from Edge Impulse as:

- **TFLite model** for Android / Flutter integration  
  (downloaded `model.tflite` and `labels.txt`)

The exported files are stored in:  

- `edge-impulse/model.tflite`  
- `edge-impulse/labels.txt`

---

## 5. Android App

### 5.1 Features

- Capture a new photo from the camera, or select from gallery
- Run on-device inference using the Edge Impulse model
- Display:
  - Predicted class (`normal` or `problem`)
  - Confidence score
- Simple UI optimized for quick use in the field

### 5.2 Architecture (short)

- `mobile-app/lib/` – main Flutter code (or `app/src/main/java` for native)
- Core pieces:
  - **Model loading:** initializes TFLite / Edge Impulse model on app start
  - **Image preprocessing:** resize and normalize to match the impulse settings
  - **Inference:** run the model and map output to labels
  - **UI:** screen for capture, preview, and result display

---

## 6. How to Run the Project

### 6.1 Requirements

- Android Studio / VS Code with Flutter SDK installed
- Android SDK and at least one emulator or a physical Android device

### 6.2 Steps (Flutter)

```bash
git clone https://github.com/VicEmmanuel/fish_pond_health_detector_edge.git
cd fishpond-health-detector-edge/mobile-app/fishpond_edge

# Install dependencies
flutter pub get

# Run on connected device
flutter run
