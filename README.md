# 🍅 Ingredient Recognition – Late Plate AI Models

This branch contains the object detection model used to identify food ingredients in real-time from user-captured images. It plays a key role in the **Late Plate** app by enabling smart ingredient recognition for personalized recipe generation and inventory management.

---

## 📦 Dataset

A custom-labeled dataset was created specifically for ingredient recognition. It includes:
- **67 ingredient classes**, covering vegetables, fruits, meats, spices, etc.
- **Bounding box annotations** to localize ingredients within images
- Images curated to reflect real-world cooking environments

This dataset was designed to outperform generic object detection datasets in culinary contexts by focusing on fine-grained ingredient distinctions.

---
## 🤖 Model

### 🔹 YOLOv11s – Fine-Tuned Object Detector

A lightweight and efficient real-time object detection model adapted for mobile deployment. The model was initialized with COCO-pretrained weights and fine-tuned on the custom ingredient dataset.

**Training Details:**
- **Epochs:** 60
- **Batch Size:** 16
- **Optimizer:** Adam
- **Initial LR:** 0.001
- **Loss Function:** Combined objectness, classification, and localization losses
- **Data Augmentation:** Random scaling, horizontal flipping, color jittering
- **Train/Validation Split:** 80/20 with early stopping (patience = 5)
- **Evaluation Metrics:** mAP@0.5 and mAP@0.75

---

## 📊 Model Evaluation

| Trial Model   | Classes | Accuracy | Inference Speed (ms) |
|---------------|---------|----------|------------------------|
| YOLOv11n      | 10      | 90%      | 280                    |
| YOLOv11n      | 67      | 52%      | 280                    |
| YOLOv11s      | 67      | 85%      | 250                    |
| YOLOv11m      | 67      | 88%      | 200                    |

- **YOLOv11n** performs well on a small class set but drops with more classes.
- **YOLOv11m** provides the highest accuracy but slower inference.
- **YOLOv11s** offers the **best balance** between accuracy (85%) and real-time performance (250ms), making it ideal for mobile deployment.
  
---

## 📱 Deployment

The trained model was converted to **TensorFlow Lite (TFLite)** for use in mobile environments. This ensures:
- Low-latency on-device inference
- Efficient performance on mobile CPUs
- Real-time ingredient detection inside the Late Plate Android app

---


## 🛠️ Dependencies

Key libraries:
- `YOLOv11`
- `TensorFlow` / `TFLite`
- `OpenCV`
- `Albumentations`
- `Matplotlib`
- `NumPy`

---

## 📜 License

This project is licensed for academic and research use only.  
Please refer to the individual model or dataset licenses before redistributing or using in production.

## 📬 Contact

This model is part of the [Late Plate](https://github.com/Late-Plate)  
For questions, issues, or contributions, please open an issue or visit the main organization page.

