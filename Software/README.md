\# Software



This folder contains the main software notebooks used for bubble detection, tracking, measurement, training, calibration, and post-processing.



\---



\## Included Notebooks



\### Bubbly\_Flow\_Analysis.ipynb



Main Bubble Evaluation Tool notebook.



This notebook launches the primary GUI used to process CNTR bubble imagery. It performs:



\- video or TIFF sequence input

\- Mask R-CNN bubble segmentation

\- CNN-based detection verification

\- Kalman filter tracking

\- annotated video export

\- predicted mask video export

\- `tracked.csv` export

\- `averages.csv` export

\- still-mode and experimental spinning-mode processing



\---



\### Results.ipynb



Post-processing and results-generation notebook.



This notebook is used after the main Bubble Evaluation Tool has generated tracking outputs. It reads processed output files such as:



\- `\*\_tracked.csv`

\- `\*\_averages.csv`

\- predicted mask videos

\- filtered/annotated videos

\- optional ground-truth files



The notebook generates thesis-ready figures, summary tables, validation metrics, and batch-level comparison outputs.



Major functions include:



\- diameter and velocity distribution plots

\- velocity versus time plots

\- velocity versus diameter plots

\- 2D bubble trajectory plots

\- track-length statistics

\- centroid and diameter uncertainty estimates

\- bubble-density summaries

\- Grace diagram plotting

\- IoU validation using ground-truth and predicted mask videos

\- synthetic ground-truth comparison

\- measured-versus-true diameter and velocity plots

\- MOTA, MOTP, and IDF1 tracking metrics

\- batch summary CSV generation

\- batch overlay plots



This notebook supports both ground-truth validation cases and non-ground-truth experimental cases.



\---



\## Training/



Contains training and calibration notebooks, including:



\- `TrainModel.ipynb`

\- `TrainCNNClassification.ipynb`

\- `Calibration.ipynb`



See \[Training/README.md](Training/README.md) for details.



\---



\## templates/



Contains timestamp character templates used by the spinning-mode timestamp extraction routine.



\---



\## Notes



Some notebooks may contain local file paths from the original development environment. These paths may need to be updated before rerunning the notebooks on another computer.



Large model weights, full training datasets, and full experimental datasets are not included due to file-size limitations.

