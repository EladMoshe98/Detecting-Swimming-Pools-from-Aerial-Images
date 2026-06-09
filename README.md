______           _  ______ _           _           
| ___ \         | | |  ___(_)         | |          
| |_/ /__   ___ | | | |_   _ _ __   __| | ___ _ __ 
|  __/ _ \ / _ \| | |  _| | | '_ \ / _` |/ _ \ '__|
| | | (_) | (_) | | | |   | | | | | (_| |  __/ |   
\_|  \___/ \___/|_| \_|   |_|_| |_|\__,_|\___|_|   
                                                   
                                                                                                                                       

# Aerial Swimming Pool Detector

Fine-tunes and benchmarks **9 object detection models** on an aerial imagery dataset to detect swimming pools from satellite/drone images. Covers CNN-based (YOLO26), transformer-based (RF-DETR), and oriented bounding box (YOLO11-OBB) architectures, with a full comparison of accuracy, speed, and localization precision.

---

## Models Benchmarked

| Model | mAP@50 | mAP@50-95 | Precision | Recall | Params (M) | Inference (ms) |
|---|---|---|---|---|---|---|
| YOLO26n | 0.500 | 0.303 | 0.626 | 0.554 | 2.38 | 39.79 |
| YOLO26s | 0.559 | 0.338 | 0.607 | 0.508 | 9.47 | 17.92 |
| YOLO26m | 0.550 | 0.349 | 0.610 | 0.569 | 20.35 | 23.70 |
| YOLO26l | 0.568 | 0.350 | 0.602 | 0.615 | 24.75 | 27.59 |
| YOLO26x | 0.509 | 0.344 | 0.698 | 0.446 | 55.63 | 57.35 |
| RF-DETR Base | 0.563 | 0.359 | 0.735 | 0.546 | — | 24.01 |
| RF-DETR Large | 0.640 | 0.370 | 0.714 | 0.606 | — | 25.98 |
| **YOLO11n-OBB** | **0.715** | **0.466** | **0.850** | **0.677** | 2.65 | 41.93 |
| **YOLO11s-OBB** | **0.739** | **0.494** | **0.765** | **0.739** | 9.70 | 35.46 |

> **Winner: YOLO11s-OBB** — best mAP@50 (0.739), mAP@50-95 (0.494), and recall (0.739), with a compact 9.7M parameter footprint.

---

## Key Findings

- **Oriented bounding boxes win**: YOLO11-OBB models outperform all axis-aligned detectors by a large margin (~+18 mAP@50 vs. the next best). Pools in aerial imagery appear at arbitrary angles, making OBB a natural fit.
- **Transformers beat CNNs at the same scale**: RF-DETR Large (0.640 mAP@50) outperforms all YOLO26 variants despite comparable inference speed.
- **YOLO26x disappoints**: The largest YOLO26 model has the worst recall (0.446), suggesting it overfits or underperforms on this relatively small aerial dataset.
- **Efficiency sweet spot**: YOLO11n-OBB achieves 0.715 mAP@50 with only 2.65M parameters — a strong choice when deployment resources are constrained.

---

## Dataset & Annotation

The dataset consists of aerial/satellite images annotated for swimming pool detection. Ground-truth labels were generated using **GroundingDINO** (open-vocabulary detection), then refined for training. Data augmentation configuration is available in [`analysis_export/augmentation_config.json`](analysis_export/analysis_export/augmentation_config.json).

---

## Repository Structure

```
.
├── Elad Moshe - CV Pool Finder (4).ipynb   # Main notebook: training, evaluation, visualisation
├── analysis_export/
│   ├── metrics_all.csv                     # Per-model benchmark results
│   └── augmentation_config.json            # Augmentation pipeline config
├── failure_cases/                          # Annotated failure images (FP, FN, double detections)
└── LICENSE
```

---

## Getting Started

```bash
# Clone the repo
git clone https://github.com/EladMoshe98/Detecting-Swimming-Pools-from-Aerial-Images.git
cd Detecting-Swimming-Pools-from-Aerial-Images

# Install dependencies (example with pip)
pip install ultralytics rfdetr supervision groundingdino-py

# Open the notebook
jupyter notebook "Elad Moshe - CV Pool Finder (4).ipynb"
```

---

## Failure Analysis

The [`failure_cases/`](failure_cases/) directory contains annotated examples of the most common error types:

- **False positives** — non-pool structures (e.g. courtyards, reflective roofs) incorrectly flagged
- **False negatives** — pools missed due to occlusion, unusual colour, or low contrast
- **Double detections** — same pool detected twice by overlapping bounding boxes

---

## License

[MIT](LICENSE)
