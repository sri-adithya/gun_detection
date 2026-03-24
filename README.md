# 🔫 Gun Detection using YOLOv5

A real-time gun detection system built on top of [Ultralytics YOLOv5](https://github.com/ultralytics/yolov5), trained on a custom annotated dataset. This project is intended for **research and safety applications** such as surveillance systems, threat detection, and public safety monitoring.

---

## 📌 Project Overview

This project fine-tunes a pre-trained YOLOv5s model on a custom gun dataset to detect firearms in images and video streams. Annotations are converted from Pascal VOC (XML) format to YOLO format as part of the preprocessing pipeline.

---

## 🗂️ Dataset Structure

The dataset follows a standard directory layout:

```
gun_dataset/
└── adithya_gun data_set/
    ├── images/
    │   ├── train/
    │   └── val/
    ├── annotations/
    │   ├── train/        ← Pascal VOC .xml files
    │   └── val/
    └── labels/           ← Auto-generated YOLO .txt files
        ├── train/
        └── val/
```

> **Note:** Annotations are provided in Pascal VOC XML format and converted to YOLO format automatically during preprocessing.

---

## ⚙️ Setup & Installation

### 1. Clone YOLOv5

```bash
git clone https://github.com/ultralytics/yolov5
cd yolov5
pip install -r requirements.txt
```

### 2. Upload and Extract Dataset

Upload your dataset ZIP file to Google Colab, then extract it:

```python
from google.colab import files
uploaded = files.upload()   # Upload your ZIP file

import zipfile
with zipfile.ZipFile("gun data_set.zip", 'r') as zip_ref:
    zip_ref.extractall("/content/gun_dataset")
```

---

## 🔄 Annotation Conversion (VOC → YOLO)

The `convert_voc_to_yolo()` function parses Pascal VOC bounding box XML files and converts them to normalized YOLO format `[class x_center y_center width height]`.

```python
def convert_voc_to_yolo(xml_file, output_txt, image_size):
    # Parses <bndbox> from XML and normalizes coordinates
    # Outputs: 0 x_center y_center width height
```

Run conversion for both splits:

```python
convert_all(f"{annotations_dir}/train", f"{labels_dir}/train")
convert_all(f"{annotations_dir}/val",   f"{labels_dir}/val")
```

---

## 📝 Data Configuration (`gun_data.yaml`)

```yaml
train: /content/gun_dataset/adithya_gun data_set/images/train
val:   /content/gun_dataset/adithya_gun data_set/images/train
nc: 1
names: ['gun']
```

> If you have a separate validation split, update the `val:` path accordingly.

---

## 🚀 Training

```bash
python train.py \
  --img 600 \
  --batch 16 \
  --epochs 49 \
  --data gun_data.yaml \
  --cfg models/yolov5s.yaml \
  --weights yolov5s.pt \
  --name gun_detector
```

| Parameter | Value | Description |
|-----------|-------|-------------|
| `--img`   | 600   | Input image size |
| `--batch` | 16    | Batch size |
| `--epochs`| 49    | Training epochs |
| `--weights` | yolov5s.pt | Pre-trained weights |
| `--name`  | gun_detector | Run name / output folder |

Training outputs are saved to `runs/train/gun_detector/`.

---

## 📊 Results

After training, evaluate your model performance using:

```bash
python val.py --weights runs/train/gun_detector/weights/best.pt --data gun_data.yaml
```

Metrics tracked: **mAP@0.5**, **Precision**, **Recall**, **F1-Score**

---

## 🔍 Inference

Run detection on a custom image or video:

```bash
python detect.py \
  --weights runs/train/gun_detector/weights/best.pt \
  --source path/to/image_or_video \
  --conf 0.4
```

---

## 📦 Export Trained Weights

To download your best model weights from Google Colab:

```python
from google.colab import files
files.download("runs/train/gun_detector/weights/best.pt")
```

---

## 🧰 Tech Stack

- [YOLOv5](https://github.com/ultralytics/yolov5) — Object detection framework
- Python 3.x
- Google Colab (GPU-accelerated training)
- Pascal VOC annotation format (XML → YOLO conversion)

---

## ⚠️ Disclaimer

This project is developed **strictly for educational and research purposes**. It is intended to assist in building safety and surveillance tools. The author does not condone or support any illegal use of this technology.

---

## 🤝 Acknowledgements

- [Ultralytics YOLOv5](https://github.com/ultralytics/yolov5) for the detection framework
- Dataset annotated and prepared by **Adithya**

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
