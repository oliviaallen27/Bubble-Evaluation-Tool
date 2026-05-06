# Thesis Cross Reference

# Overview

This document maps major sections of the thesis manuscript to the associated software, models, datasets, and documentation contained within this repository.

The repository reflects the final research software state and therefore includes additional experimental functionality beyond the finalized thesis methodology.

Primary thesis document:

```
Thesis/Olivia_Williams_Thesis.pdf
```

Defense presentation:

```
Thesis/Defense_Presentation.pptm
```

---

# Chapter 3 – Methodology

## Section 3.2 – Design of the Data Analysis Code
### Thesis Content
Overall staged processing workflow for automated bubble analysis, including:
- segmentation
- tracking
- velocity calculations
- scaling
- GUI-based analysis

### Related Repository Content

```
Software/
Documentation/user_guide.md
```

Related implementation includes:
- GUI workflow
- analysis pipeline structure
- data export logic
- processing configuration

---

## Section 3.2.1 – Bubble Segmentation and Geometric Measurements
### Thesis Content
- Mask R-CNN segmentation
- bubble mask generation
- equivalent diameter calculations
- centroid extraction
- CNN verification
- duplicate suppression
- geometric filtering

### Related Repository Content

```
Models/Mask_RCNN/
Models/CNN/
Software/
```

Related implementation includes:
- Detectron2 inference
- segmentation filtering
- mask processing
- equivalent diameter calculations
- OpenCV image moments
- CNN probability thresholding
- duplicate mask rejection



---

## Section 3.2.2 – Bubble Tracking and Identity Management
### Thesis Content
- multi-object tracking
- identity preservation
- track continuity
- assignment logic
- occlusion handling

### Related Repository Content

```
Software/
```

Related implementation includes:
- Kalman filtering
- Hungarian assignment
- Mahalanobis gating
- merge/split handling
- track confirmation logic
- track persistence filtering



---

## Section 3.2.3 – Velocity Computation and Data Export
### Thesis Content
- centroid displacement calculations
- velocity estimation
- CSV export

### Related Repository Content

```
Software/
```

Related implementation includes:
- frame-to-frame velocity calculations
- TIFF-sequence timing
- CSV generation
- tracked.csv export
- averages.csv export

:contentReference[oaicite:3]{index=3}

---

## Section 3.2.4 – Physical Scaling and Dimensionless Parameters
### Thesis Content
- spatial calibration
- physical unit conversion
- Reynolds number
- Weber number
- Eötvös number
- Bond number
- Froude number
- Morton number
- Galilei number

### Related Repository Content

```
Software/
Documentation/user_guide.md
```

Related implementation includes:
- pixel-to-inch scaling
- fluid property definitions
- dimensionless parameter calculations
- physical velocity calculations



---

## Section 3.2.5 – Graphical User Interface
### Thesis Content
- GUI design
- user inputs
- workflow controls
- output configuration

### Related Repository Content

```
Software/
Documentation/user_guide.md
```

Related implementation includes:
- Tkinter GUI
- parameter entry fields
- mode controls
- output selection
- progress tracking


---

# Section 3.3 – Synthetic Dataset Development
### Thesis Content
- synthetic bubble generation
- parameterized bubble shapes
- synthetic motion generation
- validation datasets

### Related Repository Content

```
Synthetic_Data/
```

Related implementation includes:
- synthetic image generation
- wobble bubble generation
- ellipse generation
- multi-bubble scene generation
- training and validation datasets

---

# Chapter 4 – Validation and Results

## Velocity and Tracking Validation
### Thesis Content
- tracking validation
- velocity behavior
- uncertainty analysis
- bubble trajectory analysis

### Related Repository Content

```
Software/
Sample_Data/
```

Related implementation includes:
- track visualization
- centroid tracking
- velocity export
- trajectory generation
- debug overlays

---

# Additional Repository Features Not Fully Documented in Thesis

The following features were developed after the primary thesis methodology was finalized or were not fully described in the thesis manuscript.

---

## Experimental Spinning Mode

Related documentation:

```
Documentation/spinning_mode.md
```

Features include:
- timestamp extraction
- OCR/template matching
- burst-based timing
- alternating-frame correction
- apparatus stabilization
- top-bar centering
- rotational acceleration calculations



---

## TIFF Sequence Processing

The repository supports TIFF image-sequence analysis in addition to standard video processing.

---

## Binary Predicted Mask Export

The software exports predicted binary mask videos for segmentation visualization and debugging.

---

## Debug Visualization and Logging

Additional debugging functionality includes:
- debug overlay videos
- tracking diagnostics
- assignment metrics
- debug logs

---

# Notes

Large Mask R-CNN weight files are not included in the repository due to GitHub file size limitations.

The repository represents the final research software state and therefore contains development and experimental functionality extending beyond the exact implementation described in the thesis manuscript.