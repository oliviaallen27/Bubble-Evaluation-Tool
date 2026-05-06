# Bubble Evaluation Tool

Computer vision software for automated bubble detection, tracking, and measurement from CNTR experimental imagery.

Developed by Olivia Williams  
University of Alabama in Huntsville  
Propulsion Research Center

---

# Overview

The Bubble Evaluation Tool combines:

- Detectron2 Mask R-CNN bubble segmentation
- CNN-based detection verification
- Kalman filter multi-object tracking
- Video annotation and CSV export
- Dimensionless parameter calculations
- Experimental spinning-apparatus support

The software supports both standard still-image experiments and rotating-apparatus experiments.

---

# Main Features

- Video and TIFF sequence processing
- Bubble detection and segmentation
- Bubble verification using CNN classification
- Multi-object tracking
- Physical scaling and velocity calculations
- Dimensionless parameter calculations
- Annotated video export
- CSV export
- Debug visualization tools
- Experimental spinning mode support

---

# Repository Structure

## Thesis/
Thesis PDF and defense presentation.

## Software/
Main Bubble Evaluation Tool source code and GUI.

## Models/
Model configurations and CNN weights.

## Documentation/
Installation guide, user guide, spinning-mode notes, and thesis cross references.

## Synthetic_Data/
Synthetic dataset generation scripts and sample datasets.

---

# Documentation

See:
- `Documentation/user_guide.md`
- `Documentation/installation_guide.md`
- `Documentation/thesis_cross_reference.md`
- `Documentation/spinning_mode.md`

---

# Example Outputs

## Processing Workflow

![Workflow](Figures/workflow_diagram.png)

## GUI – Still Mode

![Still GUI](Figures/gui_still_mode.png)

## GUI – Spinning Mode

![Spinning GUI](Figures/gui_spinning_mode.png)

## Example Segmentation Output

![Segmentation](Figures/segmentation_example.png)

## Example Tracking Output

![Tracking](Figures/tracking_example.png)

# Notes

Large Mask R-CNN model weight files are not included due to GitHub file size limitations.

The spinning-mode functionality is an experimental extension beyond the primary thesis methodology.