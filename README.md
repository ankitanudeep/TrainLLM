# Custom Object Detection Using COCO Format (Coffee & Tea)

This guide describes how to download image data, annotate it, convert it into COCO format, and train an object detection model using YOLOv8 on Google Colab.

---

## 1. Download Images from Google OpenImages

To get high-quality images for object detection:

### Source:
[Google OpenImages](https://storage.googleapis.com/openimages/web/index.html)

### Tool Used:
[OIDv4_ToolKit GitHub Repo](https://github.com/EscVM/OIDv4_ToolKit.git)

### Steps:

```bash
# Clone the toolkit
git clone https://github.com/EscVM/OIDv4_ToolKit.git
cd OIDv4_ToolKit

# Install dependencies
pip install -r requirements.txt

# Download images and annotations
python main.py downloader --classes Coffee Tea --type_csv train --multiclass 1 --limit 800
```

- This command downloads up to **800 images** for each class (`Coffee` and `Tea`) under:
  ```
  OpenImages/OIDv4_ToolKit/OID/Dataset/train/Coffee_Tea
  ```

- **Bounding box annotations** are downloaded under the `Label` folder.  
  Each label file corresponds to an image with the same name.

---

## 2. Annotate or Edit Labels Using CVAT

To improve or correct annotations, use [CVAT](https://www.cvat.ai/) – a web-based image annotation tool.

### Steps:

1. Create a **New Project**
2. Add labels:
   - `Coffee`
   - `Tea`
3. Upload the downloaded images.
4. Annotate each image by drawing bounding boxes and assigning the correct label.
5. After finishing, **Export the dataset** in **COCO 1.0 format**.

You’ll get an output file like:
```
instances_train.json
```

This JSON file contains all annotations (bounding boxes, class IDs, etc.) in **COCO format**.

---

## 3. Train YOLOv8 on COCO Annotations

Now that annotation is complete, move to model training.

Refer to the notebook: **`Coco_Training.ipynb`**  
It includes:

- Google Drive integration
- Dataset structure setup
- `ultralytics` installation
- Training YOLOv8 using `instances_train.json` and `instances_val.json`

---

### Dataset Structure for Training

Your dataset folder should look like this:

```
dataset/
├── images/
│   ├── train/
│   └── val/
├── annotations/
│   ├── instances_train.json
│   └── instances_val.json
├── google_colab_config.yaml
```

And the YAML config file:

```yaml
path: /content/drive/MyDrive/Colab Notebooks/Train/dataset
train: images/train
val: images/val

names:
  0: coffee
  1: tea
```

---

## Final Result

Once training is done, the YOLO model will be saved under:

```
runs/detect/train/weights/best.pt
```

You can use this model for real-time inference or deployment.
