# Sample Data

This directory contains representative example inputs and outputs for the Bubble Evaluation Tool.

---

## Input/

Contains a short example experimental input video.

---

## Output/

Contains representative processed outputs generated from the sample input, including:

- annotated tracking video
- predicted mask video
- `tracked.csv`
- `averages.csv`

These files are intended for demonstration and validation purposes only.

Large experimental datasets are not included in the repository due to file-size limitations.

---

## Training Sample Data

This folder shows an example of the expected training-data organization for the Bubble Evaluation Tool.

These files are included only as representative examples of dataset structure and formatting. They are not the complete training datasets used for the thesis, and they are not intended to fully reproduce model training by themselves.

Full training datasets are excluded from the repository due to file-size limitations.

---

## Expected Mask R-CNN Training Structure

Mask R-CNN training expects image files paired with COCO-format annotation files.

Example local structure:

```text
Training_Data/
├── Mask_RCNN/
│   ├── train_images/
│   ├── val_images/
│   ├── train_annotations.json
│   └── val_annotations.json