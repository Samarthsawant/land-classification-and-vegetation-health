<div align="center">

```
████████╗███████╗██████╗ ██████╗  █████╗ ██╗   ██╗██╗ █████╗ 
╚══██╔══╝██╔════╝██╔══██╗██╔══██╗██╔══██╗██║   ██║██║██╔══██╗
   ██║   █████╗  ██████╔╝██████╔╝███████║██║   ██║██║███████║
   ██║   ██╔══╝  ██╔══██╗██╔══██╗██╔══██║╚██╗ ██╔╝██║██╔══██║
   ██║   ███████╗██║  ██║██║  ██║██║  ██║ ╚████╔╝ ██║██║  ██║
   ╚═╝   ╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝  ╚═══╝  ╚═╝╚═╝  ╚═╝
```

**Satellite Imagery Intelligence — Land Classification & Vegetation Health Analysis**

[![Model Accuracy](https://img.shields.io/badge/Accuracy-96.2%25-brightgreen?style=for-the-badge)](/)
[![F1 Score](https://img.shields.io/badge/F1%20Score-94.8%25-green?style=for-the-badge)](/)
[![Dataset](https://img.shields.io/badge/Training%20Images-27K%2B-blue?style=for-the-badge)](/)
[![Classes](https://img.shields.io/badge/Land%20Classes-8%2B-orange?style=for-the-badge)](/)

*BT23ECE001 · BT23ECE003 — Machine Learning Course Project*

</div>

---

## 🛰️ Overview

**TerraVia** is an end-to-end machine learning platform for satellite imagery analysis. Upload a `.tif` satellite image and get instant AI-powered insights — land use classification across 8+ categories and a real-time vegetation health index — all through a sleek, browser-based interface backed by a deployed deep learning model.
https://github.com/Samarthsawant/land-classification-and-vegetation-health/releases/download/v1/terravia_v2.html
> Manual land analysis used to take weeks. TerraVia does it in under 3 seconds.

---

## 🌍 The Problem

| Challenge | Scale |
|-----------|-------|
| Global land cover has changed | **77%** of Earth's land area affected |
| Degraded land worldwide | **3.2 billion** hectares |
| Manual satellite analysis time | **~3 weeks** per region |
| Traditional methods | Expensive, unscalable, expert-dependent |

Remote sensing and land monitoring are critical for climate science, agriculture, urban planning, and disaster response — yet analysis at scale remains inaccessible without expensive software and domain expertise. TerraVia solves this.

---

## ✨ Features

- 🗺️ **Land Use Classification** — Classifies satellite imagery into 8+ land cover categories 
- 🌿 **Vegetation Health Index** — Real-time NDVI-proxy score with stress classification (Healthy / Moderate Stress / High Stress)
- 📊 **Spectral Breakdown** — Top-3 class probability scores for every prediction
- 🖼️ **Native TIFF Rendering** — Custom pure-JS TIFF parser with contrast stretching for 8-bit and 16-bit satellite imagery — no plugins needed
- ⚡ **< 3 Second Inference** — Powered by a deployed Hugging Face Space backend
- 📥 **Export Results** — Download full predictions as JSON
- 📱 **Fully Responsive** — Works on desktop and mobile

---

## 🧠 Model & Methodology

### Architecture
```
Raw Satellite Image (GeoTIFF)
         ↓
  Preprocessing & Normalization
         ↓
  Convolutional Neural Network (CNN)
  ┌─────────────────────────────┐
  │  Conv Blocks → BatchNorm    │
  │  MaxPooling → Dropout       │
  │  Dense Layers → Softmax     │
  └─────────────────────────────┘
         ↓
  Multi-class Classification + Confidence Scores
         ↓
  Vegetation Health Computation
```

### Training Pipeline
1. **Data Ingestion** — EuroSAT + IndiaSAT imagery loaded and validated
2. **Preprocessing** — Normalization, augmentation (flips, rotations, spectral jitter)
3. **CNN Training** — Multi-class classification with cross-entropy loss
4. **Evaluation** — Per-class accuracy, F1, precision, recall on held-out test set
5. **Deployment** — Exported model served via Hugging Face Spaces API

---

## 📦 Datasets

| Dataset | Images | Coverage | Bands |
|---------|--------|----------|-------|
| **EuroSAT** | ~27,000 | Europe (Sentinel-2) | 13 spectral bands |

Both datasets span diverse biomes, seasons, and resolutions — making the model robust to geographic and atmospheric variation.

---

## 📈 Results

| Metric | Score |
|--------|-------|
| **Overall Accuracy** | 96.2% |
| **F1 Score (macro)** | 94.8% |
| **Forest Precision** | 97.1% |
| **Urban Recall** | 91.3% |

### Per-Class Accuracy

```
Forest        ████████████████████ 97.1%
Water         ███████████████████  95.8%
Agriculture   ██████████████████   93.4%
Shrubland     █████████████████    91.7%
Urban         █████████████████    91.3%
Barren        ████████████████     89.5%
Wetland       ███████████████      87.2%
Snow/Ice      ██████████████████   94.0%
```

---

## 🚀 Getting Started

### Use the Live App
Visit the deployed TerraVia interface, upload any `.tif` satellite image, and get instant results.

> Make sure the Hugging Face Space backend is in **Running** state before analyzing.

### Run Locally

```bash
# follow the download link
https://github.com/Samarthsawant/land-classification-and-vegetation-health/releases/download/v1/terravia_v2.html

# Open the frontend
open terravia_v2.html
```

### Backend (Hugging Face Space)
The model is deployed at:
```
https://samdoesitbetter-terravia-api.hf.space/predict
```
Send a `POST` request with a `multipart/form-data` body containing your image file under the key `file`.

```python
import requests

with open("image.tif", "rb") as f:
    response = requests.post(
        "https://samdoesitbetter-terravia-api.hf.space/predict",
        files={"file": f}
    )

print(response.json())
# {
#   "label": "Forest",
#   "confidence": 97.1,
#   "vegetation_health": 84,
#   "all_scores": { "Forest": 97.1, "Agriculture": 1.8, ... }
# }
```

---

## 🗂️ Project Structure

```
terravia/     
├── terravia_v2.html       # Frontend — full single-file app
├── model/
│   ├── train.py           # Training script
│   ├── evaluate.py        # Evaluation & metrics
│   └── model.pt           # Trained weights
├── backend/
│   └── app.py             # FastAPI inference server (HF Space)
├── assets/
│   └── sample_images/     # Test satellite imagery
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML/CSS/JS — zero dependencies |
| TIFF Rendering | Custom pure-JS parser (8/16-bit, RGB/grayscale) |
| ML Framework | PyTorch / TensorFlow |
| Model Serving | Hugging Face Spaces (FastAPI) |
| Datasets | EuroSAT |

---

## 🔮 Future Work

- [ ] **Semantic Segmentation** — Pixel-level land cover maps instead of image-level classification
- [ ] **Temporal Analysis** — Change detection over time using multi-date image pairs
- [ ] **Multi-Spectral Fusion** — Full 13-band Sentinel-2 processing (currently RGB+NIR)
- [ ] **Mobile App** — React Native client for field surveys with camera integration

---

## 👨‍💻 Authors

| Student ID | Role |
|------------|------|
| **BT23ECE001** | ML Model, Training Pipeline, Dataset Curation |
| **BT23ECE003** | Frontend Development, Backend Deployment, UI/UX |

*Machine Learning Course Project — ECE Department*

---

<div align="center">

**Built with 🛰️ and a lot of satellite images**

</div>
