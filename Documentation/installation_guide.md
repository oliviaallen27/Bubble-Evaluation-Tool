# Installation Guide

# Overview

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

# Recommended Environment

## Software

Recommended setup:

- Anaconda
- Jupyter Notebook
- Python 3.10
- NVIDIA GPU with CUDA support (optional but recommended)

---

# Install Anaconda

Download and install:

https://www.anaconda.com/download

Install using default settings.

---

# Create Conda Environment

Open:

```
Anaconda Prompt
```

Create environment:

```
conda create -n bubbletracker python=3.10
```

Activate environment:

```
conda activate bubbletracker
```

---

# Install Jupyter Notebook

```
conda install jupyter
```

Launch Jupyter:

```
jupyter notebook
```

---

# Install PyTorch

Install the PyTorch version compatible with your CUDA version.

Example:

```
pip install torch torchvision torchaudio
```

For CUDA-specific installations, refer to:

https://pytorch.org/get-started/locally/

---

# Install Detectron2

Detectron2 installation depends on:
- operating system
- CUDA version
- PyTorch version

Recommended installation instructions:

https://detectron2.readthedocs.io/

Example:

```
pip install 'git+https://github.com/facebookresearch/detectron2.git'
```

---

# Install Additional Packages

Install required libraries:

```
pip install opencv-python
pip install numpy
pip install pandas
pip install scipy
pip install filterpy
pip install pillow
```

Tkinter is typically included with standard Python installations.

---

# Repository Setup

Clone repository:

```
git clone https://github.com/YOUR_USERNAME/Bubble-Evaluation-Tool.git
```

Move into repository:

```
cd Bubble-Evaluation-Tool
```

---

# Model Files

Large Mask R-CNN model weight files are not included in the repository due to GitHub size limitations.

Place required model files into the appropriate directories before running the software.

Expected structure:

```
Models/
├── Mask_RCNN/
└── CNN/
```

The Detectron2 `config.yaml` file must exist in the same folder as the Mask R-CNN weights.

---

# Running the Software

Launch Jupyter Notebook:

```
jupyter notebook
```

Open the notebook containing the Bubble Tracker GUI and run first code cell.

The GUI window will appear after execution.

---

# Typical Workflow

1. Launch Jupyter Notebook
2. Open Bubble Tracker notebook
3. Run all notebook cells
4. Select input data and model weights
5. Configure analysis settings
6. Run tracking
7. Review exported outputs

---

# Notes

The software was primarily developed and tested on Windows systems using Anaconda environments.

GPU acceleration is strongly recommended for Mask R-CNN inference.

Spinning-mode functionality is experimental and may require additional tuning depending on the acquisition configuration.