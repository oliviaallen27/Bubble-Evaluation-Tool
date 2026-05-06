# Sample Data

This directory contains representative example inputs and outputs for the Bubble Evaluation Tool.

## Input/

Contains a short example experimental input video.

## Output/

Contains representative processed outputs generated from the sample input, including:

* annotated tracking video
* predicted mask video
* tracked.csv
* averages.csv

These files are intended for demonstration and validation purposes only.

Large experimental datasets are not included in the repository due to file size limitations.



\## Training Data



Full training datasets are not included in this repository due to file-size limitations.



The Mask R-CNN training notebook expects a COCO-format dataset with matching image files and annotation JSON files. The annotation file is not useful without the corresponding full image set, so partial training annotations are intentionally not included.



The CNN classifier training notebook expects positive and negative cropped image folders.



Expected local structure:



```text

Training\_Data/

├── Mask\_RCNN/

│   ├── train\_images/

│   ├── val\_images/

│   ├── train\_annotations.json

│   └── val\_annotations.json

│

└── CNN\_Classification/

&#x20;   ├── train/

&#x20;   │   ├── positive/

&#x20;   │   └── negative/

&#x20;   └── val/

&#x20;       ├── positive/

&#x20;       └── negative/

