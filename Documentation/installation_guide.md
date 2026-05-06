# Installation Guide

## Overview

The Bubble Evaluation Tool was developed and tested using:

- Anaconda
- Jupyter Notebook
- Python 3.10
- CUDA-enabled PyTorch
- Detectron2

The software combines:

- Detectron2 Mask R-CNN
- PyTorch CNN classification
- OpenCV image processing
- Kalman filter tracking
- Tkinter GUI components

---

## Recommended Environment

Recommended setup:

- Windows
- Anaconda
- Jupyter Notebook
- Python 3.10
- NVIDIA GPU with CUDA support (optional but recommended)

---

## Install Anaconda

Download and install Anaconda:

https://www.anaconda.com/download

Install using default settings.

---

## Create Conda Environment

Open Anaconda Prompt.

Create the environment:

```bash
conda create -n bubbletracker python=3.10
```

Activate the environment:

```bash
conda activate bubbletracker
```

---

## Install Jupyter Notebook

```bash
conda install jupyter ipykernel
```

Register the environment as a Jupyter kernel:

```bash
python -m ipykernel install --user --name bubbletracker --display-name "Python (bubbletracker)"
```

Launch Jupyter:

```bash
jupyter notebook
```

---

## Install PyTorch

Install the PyTorch version compatible with your CUDA version.

Example CPU/default install:

```bash
pip install torch torchvision torchaudio
```

For CUDA-specific installations, refer to:

https://pytorch.org/get-started/locally/

---

## Install Detectron2

Detectron2 installation depends on:

- operating system
- CUDA version
- PyTorch version

Recommended installation instructions:

https://detectron2.readthedocs.io/

Example:

```bash
pip install "git+https://github.com/facebookresearch/detectron2.git"
```

---

## Install Additional Packages

Install required libraries:

```bash
pip install opencv-python numpy pandas scipy filterpy pillow
```

Tkinter is typically included with standard Python installations.

---

## Repository Setup

Clone the repository:

```bash
git clone https://github.com/oliviaallen27/Bubble-Evaluation-Tool.git
```

Move into the repository:

```bash
cd Bubble-Evaluation-Tool
```

---

## Model Files

Large Mask R-CNN model weight files are not included in the repository due to GitHub file-size limitations.

Place required Mask R-CNN model files into the appropriate local directories before running the software.

Expected structure:

```text
Models/
├── Mask RCNN/
│   ├── Water/
│   ├── SPT3/
│   └── Synthetic/
└── CNN/
    ├── Water/
    ├── SPT3/
    └── Synthetic/
```

Each Mask R-CNN model folder should contain:

```text
model_final.pth
config.yaml
```

The Detectron2 `config.yaml` file must exist in the same folder as the Mask R-CNN weights.

---

## Running the Software

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open:

```text
Software/Bubbly_Flow_Analysis.ipynb
```

Select the `Python (bubbletracker)` kernel.

Run the main GUI code cell.

The GUI window will appear after execution.

---

## Typical Workflow

1. Launch Jupyter Notebook.
2. Open the Bubble Tracker notebook.
3. Run the main GUI code cell.
4. Select input data and model weights.
5. Configure analysis settings.
6. Run tracking.
7. Review exported outputs.

---

## Notes

The software was primarily developed and tested on Windows systems using Anaconda environments.

GPU acceleration is strongly recommended for Mask R-CNN inference.

Spinning-mode functionality is experimental and may require additional tuning depending on the acquisition configuration.