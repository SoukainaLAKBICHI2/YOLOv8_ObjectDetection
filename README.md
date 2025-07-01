# Assistive Danger Detection System for the Visually Impaired 👁️🦯

This project implements a real-time danger detection system using a custom-trained YOLOv8 model. The system detects objects representing potential hazards for visually impaired users, estimates their distance, and prioritizes audio alerts to enhance safety during navigation.

## 📌 Project Objectives

- Detect objects representing urban dangers.
- Classify detected objects into "High Danger" and "Regular Danger".
- Estimate distance to each object using bounding box information.
- Prioritize alerts: dangerous objects are announced first, even if slightly farther away.
- Manage alert frequency to avoid overwhelming the user with repetitive announcements.


## 📷 Dataset Creation

### Why a Custom Dataset?
Existing datasets didn’t include all 25 classes of interest (e.g., open manholes, fallen signs). We built a dataset covering specific hazards by downloading and labeling images manually.

- Images collected from Kaggle, Roboflow, and Open Images.
- Created our own dataset for rare/contextual dangers.
- Annotations performed with [LabelImg](https://github.com/heartexlabs/labelImg) in YOLO format.

### Dataset Split
- **80%** images for training.
- **20%** images for validation.


## ⚙️ Training the YOLOv8 Model

- **Base model**: `yolov8m.pt` pretrained weights.
- **Training duration**: 150 epochs.
- **Optimizer**: AdamW with low learning rate for stability.
- **Data augmentation**:
  - Rotations, translations, zoom, shear.
  - Mosaics, mixup, copy-paste for stronger generalization.
  - Color augmentations for lighting variations.

- **Regularization**:
  - Weight decay
  - Dropout
- **Validation**:
  - Automatic evaluation at each epoch.
  - Checkpoints saved every 25 epochs.


## 📈 Training Results

The model achieved:
- **Precision**: 73.95%
- **Recall**: 69.33%
- **mAP50**: 73.95%


## 📏 Distance Estimation

After detecting an object:
1. The **camera focal length** is calibrated using an object of known real-world dimensions.
2. The bounding box coordinates (center X, center Y, width, height) are used.
3. Distance to the object is calculated.


## 🔊 Alert Management

- Only objects within **5 meters** are announced.
- A minimum delay of **5 seconds** is enforced before repeating alerts for the same class.
- If a dangerous object is detected, it is prioritized for the audio alert—even if it’s a bit farther than other objects.
- A special **beep sound** warns the user before announcing a dangerous object.



## 🚦 Prioritization Logic

```text
Detect object ➔ Is it dangerous?
    └─ No: announce closest object.
    └─ Yes: announce closest danger first (with beep), even if slightly farther.
```

## 🚀 How to Run
1️⃣ Clone the repo:
```
git clone https://github.com/yourusername/Yolov8_ObjectDetection.git
cd Yolov8_ObjectDetection
```

2️⃣ Install dependencies:

```
pip install -r requirements.txt
```

3️⃣ Train or test the model


## 👩‍💻 Author
Build with passion and ❤️ by 👩‍💻 Soukaina LAKBICHI

