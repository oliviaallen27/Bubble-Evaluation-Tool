# Training and Calibration Code

This folder contains supporting notebooks used to train, calibrate, and validate components of the Bubble Evaluation Tool.

The notebooks are included for documentation, reproducibility, and future development. Some local file paths may need to be updated before rerunning them on another computer.

---

## Included Notebooks

### TrainModel.ipynb

Mask R-CNN training notebook using Detectron2.

This notebook includes:

- CUDA/GPU environment checks
- Detectron2 imports and setup
- COCO-format dataset registration
- Mask R-CNN model configuration
- training setup
- custom data augmentation / mapper logic
- model training
- model configuration export
- validation and visualization utilities

Expected outputs may include:

- `model_final.pth`
- `config.yaml`
- training metrics
- validation visualizations

Large Mask R-CNN weight files are not included in this repository due to GitHub file-size limitations.

---

### TrainCNNClassification.ipynb

CNN classifier training notebook for secondary bubble verification.

This notebook trains a binary image classifier used to distinguish valid bubble detections from false positives or non-bubble regions.

This notebook includes:

- PyTorch model setup
- ResNet-based binary classification
- training and validation data loading
- image augmentation and normalization
- model training
- model saving
- prediction/testing utilities
- conversion between full pickled model files and state-dict style weights
- crop generation utilities for positive and negative training images

Expected outputs may include:

- `bubble_classifier.pth`
- `best_model_weights.pth`
- validation predictions
- cropped training images

---

### Calibration.ipynb

Synthetic bubble generation and calibration-support notebook.

This notebook generates synthetic bubble videos and supporting ground-truth data used for testing, calibration, and validation of the bubble analysis pipeline.

Synthetic cases include:

- wobbly ellipse bubbles
- perfect ellipse bubbles
- perfect circle bubbles
- spinning bubble examples
- multi-bubble examples
- merged/multi-bubble scenes
- synthetic training data generation

The notebook includes support for:

- Phantom-style timestamp overlays
- template-based timestamp graphics
- bubble mask generation
- centroid and bounding-box extraction
- synthetic ground-truth CSV output
- controlled bubble shape and motion generation

---

## Training Data

Full training datasets are not included in this repository due to file-size limitations.

The Mask R-CNN training notebook expects a COCO-format dataset with matching image files and annotation JSON files. The annotation file is not useful without the corresponding full image set, so partial training annotations are intentionally not included.

The CNN classifier training notebook expects positive and negative cropped image folders.

Expected local structure:

```text
Training_Data/
├── Mask_RCNN/
│   ├── train_images/
│   ├── val_images/
│   ├── train_annotations.json
│   └── val_annotations.json
│
└── CNN_Classification/
    ├── train/
    │   ├── positive/
    │   └── negative/
    └── val/
        ├── positive/
        └── negative/
```

Large datasets should be stored separately on external storage, institutional storage, OneDrive, Google Drive, or a lab server.

---

## Notes

Large datasets, intermediate training outputs, TensorBoard logs, and large model weights are not included in this repository.

The notebooks preserve the development workflow used to support the thesis methodology, but some file paths are local to the original development computer and may need to be updated before reuse.

The trained Mask R-CNN weights are excluded because they exceed GitHub's standard file-size limit. Model folders contain README files describing where these files should be placed locally.