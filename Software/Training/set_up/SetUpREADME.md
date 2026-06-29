# Training Setup Notebooks

This folder contains setup notebooks used to prepare image datasets for training the Bubble Evaluation Tool models.

These notebooks do **not** train the models directly. They prepare the image files, MATLAB label masks, COCO annotations, and cropped CNN image patches needed before model training.

The main training notebooks are located one folder above:

```text
Software/Training/
├── TrainModel.ipynb
└── TrainCNNClassification.ipynb
```

The setup notebooks in this folder are used before running those training notebooks.

---

## Setup Workflow

The recommended workflow is:

```text
raw images
  ↓
convert_images_to_png.ipynb
  ↓
label PNG images in MATLAB Image Labeler / Pixel Labeler
  ↓
export MATLAB pixel label masks
  ↓
convert_matlab_pixel_labels_to_coco_one_class.ipynb
     or
convert_matlab_pixel_labels_to_coco_two_class.ipynb
  ↓
train Mask R-CNN using TrainModel.ipynb
  ↓
crop_cnn_positive_patches_from_masks.ipynb
  ↓
create_cnn_negative_images_from_masks.ipynb
  ↓
organize CNN train/val folders
  ↓
train CNN classifier using TrainCNNClassification.ipynb
```

---

# 1. Convert Images to PNG

Notebook:

```text
convert_images_to_png.ipynb
```

This notebook converts raw training images into ordered `.png` files with consistent names.

Example output:

```text
bubble_000001.png
bubble_000002.png
bubble_000003.png
filename_map.csv
```

The `filename_map.csv` file records the original filename and the new PNG filename.

Use this notebook before labeling images in MATLAB. Do not rename the images after labeling, because the MATLAB-exported mask filenames must match the image filenames.

---

# 2. Label Images in MATLAB

After converting the images to PNG, open the PNG image folder in MATLAB Image Labeler or Pixel Labeler.

For a one-class detector, use one label:

```text
bubble
```

For a two-class detector, use two labels:

```text
attached_bubble
detached_bubble
```

After labeling, export the pixel label data from MATLAB.

The exported folder is usually named something like:

```text
PixelLabelData/pixelLabelData
```

The exported mask files may have MATLAB-style names such as:

```text
Label_1_bubble_000001.png
Label_2_bubble_000002.png
```

The COCO conversion notebooks are designed to handle this naming pattern.

---

# 3. Convert MATLAB Labels to COCO Format

These notebooks convert MATLAB-exported pixel label masks into COCO-format annotation files for Detectron2 Mask R-CNN training.

The output files are:

```text
train.json
val.json
```

These files are used by `TrainModel.ipynb`.

---

## One-Class COCO Conversion

Notebook:

```text
convert_matlab_pixel_labels_to_coco_one_class.ipynb
```

Use this notebook when all bubbles are labeled as a single class:

```text
bubble
```

Expected indexed MATLAB mask format:

```text
0 = background
1 = bubble
```

Output COCO category:

```text
category_id 1 = bubble
```

Use this format when the Mask R-CNN model should detect bubbles without distinguishing between attached and detached bubbles.

---

## Two-Class COCO Conversion

Notebook:

```text
convert_matlab_pixel_labels_to_coco_two_class.ipynb
```

Use this notebook when bubbles are labeled as two classes:

```text
attached_bubble
detached_bubble
```

Expected indexed MATLAB mask format:

```text
0 = background
1 = attached_bubble
2 = detached_bubble
```

Output COCO categories:

```text
category_id 1 = attached_bubble
category_id 2 = detached_bubble
```

Use this format when the Mask R-CNN model should distinguish between attached bubbles near the injection region and detached bubbles in the flow field.

---

# 4. Crop Positive CNN Patches

Notebook:

```text
crop_cnn_positive_patches_from_masks.ipynb
```

This notebook creates positive CNN training examples from labeled bubble masks.

It loads each full-size image and its matching MATLAB mask, finds each connected bubble region, computes the centroid of the region, and saves a fixed-size crop centered on the bubble.

Default patch size:

```text
128 x 128 pixels
```

Example output:

```text
bubble_000001_1.png
bubble_000001_2.png
bubble_000002_1.png
```

These patches should be used as positive examples for the CNN classifier and placed into a folder such as:

```text
cnn_dataset/train/bubble/
cnn_dataset/val/bubble/
```

---

# 5. Create Negative Images from Masks

Notebook:

```text
create_cnn_negative_images_from_masks.ipynb
```

This notebook creates cleaned negative images for the CNN classifier by removing labeled bubble regions from the original images.

It combines the labeled bubble masks, optionally dilates the masked regions, and uses OpenCV inpainting to fill the bubble areas. This creates full-frame images where the labeled bubbles have been removed.

Example output:

```text
bubble_000001.png
bubble_000002.png
bubble_000003.png
```

These cleaned images are useful for generating non-bubble examples from the same lighting, background, glare, window, and apparatus conditions as the positive bubble images.

These images are not final CNN patches by themselves. The CNN classifier expects fixed-size image patches. Negative patches should be cropped or sampled from these cleaned images and placed into folders such as:

```text
cnn_dataset/train/not_bubble/
cnn_dataset/val/not_bubble/
```

---

# 6. CNN Dataset Folder Structure

The CNN classifier training notebook uses PyTorch `ImageFolder`, so the folder names define the class labels.

Recommended structure:

```text
cnn_dataset/
├── train/
│   ├── bubble/
│   └── not_bubble/
└── val/
    ├── bubble/
    └── not_bubble/
```

The `bubble` folders should contain positive bubble patches.

The `not_bubble` folders should contain negative background or non-bubble patches.

---

# 7. Recommended Dataset Folder Structure

A complete local training dataset may look like this:

```text
training_dataset/
├── raw_images/
├── png_training_images/
├── pixelLabelData/
├── coco_annotations/
├── cnn_positive_patches/
├── cnn_negative_images/
├── cnn_negative_patches/
└── cnn_dataset/
    ├── train/
    │   ├── bubble/
    │   └── not_bubble/
    └── val/
        ├── bubble/
        └── not_bubble/
```

The output folders do not need to be stored inside the GitHub repository. The notebooks should create output folders automatically or allow the user to choose output paths.

---

# 8. Notes for BLENDER and CNTR Images

For BLENDER spin-mode data, include training examples from the same imaging conditions expected during analysis.

Useful training examples include:

```text
isolated detached bubbles
attached or near-attached bubbles
bubbles near rods
bubbles near bright window edges
small bubbles near the injection region
high-RPM blur cases
dense plume regions where individual bubbles are visually separable
```

Do not label unresolved plume texture as individual bubbles unless the boundary of each bubble can be visually defended.

At high rotational speeds, some bubbles may not be separable as individual instances because of overlap, blur, glare, and viewing angle. These cases should be treated as a limitation of the image data, not only as a model failure.

---
