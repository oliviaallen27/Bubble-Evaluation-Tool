# Thesis Cross Reference

## Overview

This document maps major sections of the thesis manuscript to the associated software, models, sample data, figures, and documentation contained within this repository.

The repository reflects the final research software state and therefore includes additional experimental functionality beyond the finalized thesis methodology.

Primary thesis document:

[Thesis/Olivia Williams Thesis.pdf](../Thesis/Olivia%20Williams%20Thesis.pdf)

The defense presentation is not included because the file exceeds GitHub's standard file-size limit.

---

## Chapter 3 – Methodology

### Section 3.2 – Design of the Data Analysis Code

#### Thesis Content

Overall staged processing workflow for automated bubble analysis, including:

- segmentation
- tracking
- velocity calculations
- physical scaling
- dimensionless parameter calculations
- GUI-based analysis

#### Related Repository Content

- [Software](../Software/)
- [User Guide](user_guide.md)

Related implementation includes:

- GUI workflow
- analysis pipeline structure
- data export logic
- processing configuration
- still-mode and spinning-mode selection

---

### Section 3.2.1 – Bubble Segmentation and Geometric Measurements

#### Thesis Content

- Mask R-CNN segmentation
- bubble mask generation
- equivalent diameter calculations
- centroid extraction
- CNN verification
- duplicate suppression
- geometric filtering

#### Related Repository Content

- [Mask R-CNN Models](../Models/Mask%20RCNN/)
- [CNN Models](../Models/CNN/)
- [Software](../Software/)
- [Segmentation Example](../Figures/segmentation_example.png)

Related implementation includes:

- Detectron2 inference
- segmentation filtering
- mask processing
- equivalent diameter calculations
- OpenCV image moments
- CNN probability thresholding
- duplicate mask rejection

---

### Section 3.2.2 – Tracking and Identity Management

#### Thesis Content

- multi-object tracking
- identity preservation
- track continuity
- assignment logic
- occlusion handling

#### Related Repository Content

- [Software](../Software/)
- [Tracking Example](../Figures/tracking_example.png)

Related implementation includes:

- Kalman filtering
- Hungarian assignment
- Mahalanobis gating
- merge/split handling
- track confirmation logic
- track persistence filtering

---

### Section 3.2.3 – Velocity Computation and Output Generation

#### Thesis Content

- centroid displacement calculations
- velocity estimation
- annotated video generation
- CSV export

#### Related Repository Content

- [Software](../Software/)
- [Sample Output Data](../Sample_Data/Output/)

Related implementation includes:

- frame-to-frame velocity calculations
- TIFF-sequence timing
- CSV generation
- `tracked.csv` export
- `averages.csv` export
- annotated video export
- predicted mask video export

---

### Section 3.2.4 – Physical Scaling and Derived Parameters

#### Thesis Content

- spatial calibration
- physical unit conversion
- Reynolds number
- Weber number
- Eötvös number
- Bond number
- Froude number
- Morton number
- Galilei number

#### Related Repository Content

- [Software](../Software/)
- [User Guide](user_guide.md)

Related implementation includes:

- pixel-to-inch scaling
- fluid property definitions
- physical velocity calculations
- dimensionless parameter calculations
- CSV output of derived quantities

---

### Section 3.2.5 / Appendix B – Software Implementation and GUI Documentation

#### Thesis Content

- GUI design
- user inputs
- workflow controls
- output configuration
- software implementation details

#### Related Repository Content

- [Software](../Software/)
- [User Guide](user_guide.md)
- [Installation Guide](installation_guide.md)
- [GUI Still Mode Example](../Figures/gui_still_example.png)
- [GUI Spinning Mode Example](../Figures/gui_spinning_example.png)

Related implementation includes:

- Tkinter GUI
- parameter entry fields
- mode controls
- output selection
- progress tracking
- Jupyter Notebook execution workflow

---

## Section 3.3 – Training and Synthetic Dataset Development

#### Thesis Content

- model training
- annotated experimental data
- synthetic bubble generation
- parameterized bubble shapes
- synthetic motion generation
- validation datasets

#### Related Repository Content

- [Models](../Models/)
- [Software](../Software/)
- [Sample Data](../Sample_Data/)

Related implementation includes:

- Mask R-CNN model configuration
- CNN model weights
- synthetic validation support
- sample input/output data

Note: Full training datasets and large generated datasets are not included due to file-size limitations.

---

## Section 3.4 – Validation and Benchmarking

#### Thesis Content

- synthetic ground-truth validation
- experimental cross-checks
- tracking validation
- physical consistency checks
- uncertainty analysis

#### Related Repository Content

- [Sample Data](../Sample_Data/)
- [Figures](../Figures/)
- [Software](../Software/)

Related implementation includes:

- tracked output CSV files
- average output CSV files
- predicted mask video export
- annotated tracking video export
- representative validation figures

---

## Chapter 4 – Results

#### Thesis Content

- detection and tracking performance
- bubble size statistics
- bubble velocity statistics
- tracking persistence
- time-resolved trajectories
- velocity versus diameter behavior
- dimensionless validation
- synthetic and hand-labeled validation

#### Related Repository Content

- [Figures](../Figures/)
- [Sample Data](../Sample_Data/)
- [Software](../Software/)

Related repository materials include:

- workflow diagram
- GUI screenshots
- representative segmentation output
- representative tracking output
- sample processed CSV files
- sample processed videos

---

## Additional Repository Features Not Fully Documented in Thesis

The following features were developed after the primary thesis methodology was finalized or were not fully described in the thesis manuscript.

---

### Experimental Spinning Mode

Related documentation:

- [Spinning Mode Notes](spinning_mode.md)

Features include:

- timestamp extraction
- OCR/template matching
- burst-based timing
- alternating-frame correction
- apparatus stabilization
- top-bar centering
- rotational acceleration calculations

Current status:

- implemented in software
- documented separately
- not yet fully validated on spinning experimental datasets

---

### TIFF Sequence Processing

The repository supports TIFF image-sequence analysis in addition to standard video processing.

---

### Binary Predicted Mask Export

The software exports predicted binary mask videos for segmentation visualization and debugging.

---

### Debug Visualization and Logging

Additional debugging functionality includes:

- debug overlay videos
- tracking diagnostics
- assignment metrics
- debug logs

---

## Notes

Large Mask R-CNN weight files are not included in the repository due to GitHub file-size limitations.

The repository represents the final research software state and therefore contains development and experimental functionality extending beyond the exact implementation described in the thesis manuscript.