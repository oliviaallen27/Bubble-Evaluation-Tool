# User Guide

# Bubble Tracker GUI

The Bubble Tracker GUI is an interactive Python/Jupyter-based application developed to perform automated bubble detection, tracking, and measurement from CNTR video data and TIFF image sequences.

The software combines:
- Detectron2 Mask R-CNN instance segmentation
- CNN-based bubble verification
- Kalman filter multi-object tracking
- Assignment-based track association
- Physical scaling and dimensionless parameter calculations

The application supports both still and spinning experimental configurations and exports annotated videos and quantitative measurement data for analysis.

---

# Operating Modes

## Still Mode

Still mode assumes a constant frame rate specified by the user.

Timing is computed using:

```
time = frame_number / FPS
```

This mode is intended for standard non-rotating experiments.

---

## Spinning Mode

Spinning mode is designed for rotating-apparatus experiments.

Features include:
- OCR/template-based timestamp extraction
- Burst-based processing
- Optional frame flipping for alternating orientation
- Apparatus stabilization using top-center bar alignment
- Effective radial acceleration calculations

Timing is extracted directly from on-screen timestamps rather than a fixed FPS value.

---

# GUI Inputs and Configuration Parameters

## Input Video

Path to the video file to be analyzed.

Supported formats include:
- `.avi`
- `.mp4`
- `.mov`
- `.mkv`
- `.m4v`

The video is processed frame-by-frame.

---

## Input TIFF Folder

Optional folder containing TIFF image sequences.

Supported formats:
- `.tif`
- `.tiff`

Images are loaded sequentially using natural filename sorting.

---

## Mask R-CNN Weights

Path to the trained Detectron2 Mask R-CNN model weights (`.pth` or `.pt`).

The corresponding `config.yaml` file must exist in the same directory and is automatically loaded during initialization.

---

## CNN Weights

Path to the trained secondary CNN classifier weights used to verify detections and suppress false positives.

---

## Output Folder

Directory where all generated outputs are written.

Outputs may include:
- Annotated videos
- Predicted mask videos
- CSV files
- Debug logs
- Debug videos

---

## Filename Prefix

Base filename used for all outputs.

Example:

```
experiment_01_filtered.mp4
experiment_01_tracked.csv
experiment_01_averages.csv
```

If left blank, the input filename is used automatically.

---

## FPS (Still Mode)

Frame rate used for timing calculations in Still mode.

Ignored in Spinning mode.

---

## Max Assign Distance (px)

Maximum allowed pixel distance between:
- predicted track position
- detected bubble centroid

during association.

---

## Max Misses (frames)

Maximum number of consecutive frames a track may remain unmatched before being terminated.

---

## Ignore Text (px)

Vertical pixel threshold used to suppress detections near on-screen text overlays.

Detections above this threshold are discarded.

---

## Bubble Diameter Limits (px)

Minimum and maximum allowable bubble diameters.

Detections outside this range are rejected.

---

## Scale (Inches per Pixel)

Optional spatial calibration used to convert:
- diameters
- velocities
- positions

from pixel units into physical units.

When provided, dimensionless parameters are computed.

---

## Liquid Selection

Working liquid used for dimensionless parameter calculations.

Currently supported:
- Water
- SPT3

Properties include:
- viscosity
- surface tension
- density

---

## Gas Selection

Gas phase used for density calculations.

Currently supported:
- Air
- Nitrogen

---

## Overlay Color

Color used for:
- masks
- outlines
- labels
- tracking overlays

in exported videos.

---

# Spinning Mode Parameters

## RPM

Rotational speed of the experimental apparatus.

Used to compute effective radial acceleration.

---

## Frames per Burst

Number of frames contained within each rotational burst.

---

## Reset IDs per Burst

When enabled:
- active tracks are cleared at the beginning of each burst
- globally unique display IDs are preserved

---

# Output Options

## Save Debug Outputs

Enables export of:
- debug videos
- debug overlays
- assignment diagnostics
- debug logs
- tracking-association metrics

---

## Save Measurement CSVs

Exports:
- `tracked.csv`
- `averages.csv`

---

# Generated Outputs

## Filtered Video

```
{prefix}_filtered.mp4
```

Final annotated video showing accepted tracks.

---

## Predicted Mask Video

```
{prefix}_pred_mask.mp4
```

Binary mask visualization of predicted bubble regions.

---

## Tracked CSV

```
{prefix}_tracked.csv
```

Contains frame-resolved measurements including:
- bubble ID
- centroid position
- diameter
- velocity
- dimensionless parameters

---

## Averages CSV

```text
{prefix}_averages.csv
```

Contains:
- per-bubble averages
- overall averages
- GUI settings used during the run

---

# Dimensionless Parameters

When scale information is available, the software computes:
- Reynolds number
- Eötvös number
- Weber number
- Froude number
- Galilei number
- Morton number
- Bond number

---

# Typical Workflow

1. Select input video or TIFF folder
2. Select model weight files
3. Choose output folder
4. Configure analysis settings
5. Select operating mode
6. Run tracking
7. Review exported videos and CSV files

---

# Software Requirements

Main dependencies include:
- Python
- Detectron2
- PyTorch
- OpenCV
- NumPy
- Pandas
- SciPy
- FilterPy
- Pillow
- Tkinter

---

# Notes

Large Mask R-CNN model weights are not included in the repository due to GitHub file size limitations.

The spinning-mode functionality is considered experimental and extends beyond the primary thesis methodology.