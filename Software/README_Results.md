# Results Post-Processing Notebook

## File

`Results.ipynb`

## Purpose

`Results.ipynb` is the post-processing and validation notebook for the Bubble Evaluation Tool. It is intended to be run after the main tracking notebook or GUI has generated bubble tracking outputs.

The notebook reads tracking CSV files, averages/settings files, optional ground-truth files, and optional output videos. It then generates figures, summary tables, validation metrics, uncertainty estimates, and batch-level comparison outputs.

The notebook supports two main workflows:

1. **Non-ground-truth analysis**, used for experimental cases where only tracked bubble outputs are available.
2. **Ground-truth validation analysis**, used for synthetic or hand-labeled cases where a matching ground-truth file is available.

The notebook automatically determines which workflow to use based on whether a matching `*_gt.csv` file exists for the selected `*_tracked.csv` file.

---

## Expected File Naming

The results notebook relies on consistent file naming. For a run named:

```text
RunName_tracked.csv
```

the notebook automatically looks in the same folder for related files with the same prefix:

```text
RunName_averages.csv
RunName_gt.csv
RunName_pred_mask.mp4
RunName_filtered.mp4
RunName_gt_mask.mp4
RunName_manual.csv
```

Only `RunName_tracked.csv` is strictly required. However, additional files enable more complete analysis.

---

## Required Input Files

### 1. Tracked CSV

```text
RunName_tracked.csv
```

This is the main tracking output file. It contains detected bubble information for each tracked bubble instance.

Expected data include:

- frame number
- bubble track ID
- centroid x-position
- centroid y-position
- equivalent bubble diameter
- bubble velocity or velocity components

The notebook maps common column-name variations automatically when possible.

Common accepted tracked CSV columns include:

| Quantity | Common column names |
|---|---|
| Frame | `frame`, `Frame` |
| Track ID | `id`, `ID`, `track_id` |
| Centroid x | `cx`, `x`, `centroid_x` |
| Centroid y | `cy`, `y`, `centroid_y` |
| Diameter | `diam_px`, `diam`, `diameter_px` |
| Speed | `vel_px_s`, `speed_px_s`, `vel`, `speed` |
| x-velocity | `vel_x_px_s`, `vx_px_s`, `vx` |
| y-velocity | `vel_y_px_s`, `vy_px_s`, `vy` |

If speed is not directly available, the notebook calculates speed from the x- and y-velocity components when available.

---

## Strongly Recommended Input Files

### 2. Averages/settings CSV

```text
RunName_averages.csv
```

The averages file stores run settings and summary values from the tracking tool. It is strongly recommended because it provides metadata needed for physical-unit conversion and physics calculations.

This file may include:

- scale calibration
- frames per second
- liquid type
- gas type
- ignored top region
- run-level averaged diameter and velocity values
- Reynolds and Eötvös number information

When a valid scale is found, the notebook converts pixel-based measurements into inches and inches per second. When no scale is found, the notebook keeps all measurements in pixels and pixels per second.

---

## Optional Input Files

### 3. Ground-truth CSV

```text
RunName_gt.csv
```

If this file exists, the notebook runs ground-truth validation mode.

The GT file is used to compare measured bubble properties against known or hand-labeled values.

Required GT data include:

| Quantity | Common column names |
|---|---|
| Frame | `frame`, `Frame` |
| GT ID | `gt_id` |
| GT centroid x | `gt_cx_px`, `gt_cx`, `cx`, `x` |
| GT centroid y | `gt_cy_px`, `gt_cy`, `cy`, `y` |
| GT diameter | `gt_diam_equiv_px`, `gt_diam_eq`, `gt_diam_px`, `diam_px`, `diam` |

Optional GT velocity data include:

| Quantity | Common column names |
|---|---|
| GT speed | `gt_speed_px_s`, `gt_speed`, `speed_px_s`, `speed` |
| GT x-velocity | `gt_vx_px_s`, `gt_vx`, `vx` |
| GT y-velocity | `gt_vy_px_s`, `gt_vy`, `vy` |

If GT velocity data are not available, the notebook still performs diameter, centroid, matching, and mask-based validation. Speed validation plots are skipped.

### 4. Predicted mask video

```text
RunName_pred_mask.mp4
```

This video contains the predicted binary mask output from the segmentation model.

It is used for:

- segmentation IoU calculation
- mask comparison panels
- qualitative validation figures

### 5. Filtered or annotated tracking video

```text
RunName_filtered.mp4
```

This video contains the final filtered or annotated tracking output.

It is used for qualitative figures showing the tracked bubbles and assigned IDs.

### 6. Ground-truth mask video

```text
RunName_gt_mask.mp4
```

This video contains the ground-truth binary masks for synthetic or labeled validation data.

It is used with `RunName_pred_mask.mp4` to compute semantic segmentation IoU and create mask-difference panels.

### 7. Optional manual comparison CSV

```text
RunName_manual.csv
```

This file can be used for an optional manual-vs-automatic comparison. It is intended for cases where a small number of manually measured bubble properties are available.

---

## Analysis Modes

### Non-Ground-Truth Mode

The notebook enters non-GT mode when a matching `RunName_gt.csv` file is not found.

This mode is used for experimental cases where the true bubble diameter, centroid, and velocity are unknown.

The purpose of this mode is to generate descriptive figures and run-level summaries from the tracked data.

Non-GT mode produces:

- diameter distributions
- velocity distributions
- representative trajectory plots
- velocity vs. diameter plots
- velocity vs. time plots
- smoothed velocity vs. time plots
- track-length statistics
- per-bubble statistics
- centroid uncertainty estimates
- diameter uncertainty estimates
- bubble-density summaries
- Grace diagram placement
- qualitative tracking panels

### Ground-Truth Validation Mode

The notebook enters GT mode when a matching `RunName_gt.csv` file is found.

In this mode, the notebook matches detected/measured bubbles to ground-truth bubbles on a frame-by-frame basis. Matching is performed using centroid distance and a user-selected maximum match distance.

GT mode produces:

- matched measured/true bubble pairs
- measured-vs-true diameter plots
- measured-vs-true speed plots, when GT speed is available
- diameter error distributions
- speed error distributions, when GT speed is available
- diameter error vs. time
- speed error vs. time, when GT speed is available
- relative diameter error distributions
- segmentation IoU results
- MOTA, MOTP, and IDF1 tracking metrics
- mask difference panels
- qualitative validation panels
- KS two-sample distribution comparisons
- standardized distribution summaries

---

## GUI Workflow

The notebook includes a Tkinter GUI called:

```text
Bubble Results Post-Processing Tool
```

The GUI contains four main sections:

1. **Single File**
2. **Batch Folder**
3. **Output Settings**
4. **Run Analysis**

### Single File

Use this option to process one `*_tracked.csv` file.

The tool automatically searches for related files in the same folder.

### Batch Folder

Use this option to process every `*_tracked.csv` file in a selected folder.

Each tracked file is processed as a separate run. The notebook also creates batch-level overlays and a master summary CSV.

### Output Settings

The output root folder is where all results are saved.

For each run, the notebook creates a new output folder named after the run prefix.

Example:

```text
OutputRoot/
    RunName/
        figs/
        tables/
        errors.log
```

### GT Match Distance

The GT match distance controls the maximum allowed centroid distance between a GT bubble and a measured bubble during frame-by-frame matching.

This value is only used when a matching `*_gt.csv` file is found.

If no GT file exists, the notebook automatically runs non-GT analysis.

---

## Output Folder Structure

For a single run, the output structure is:

```text
OutputRoot/
    RunName/
        figs/
        tables/
        errors.log
```

For batch mode, the output structure is:

```text
OutputRoot/
    RunName_1/
        figs/
        tables/
        errors.log

    RunName_2/
        figs/
        tables/
        errors.log

    master_summary.csv

    batch_overlays/
        fig_batch_overlay_diameter.png
        fig_batch_overlay_velocity.png
        fig_grace_all_runs_avg.png

        accuracy/
            fig_batch_diam_MAE.png
            fig_batch_diam_RMSE.png
            fig_batch_diam_Bias.png
            fig_batch_speed_MAE.png
            fig_batch_speed_RMSE.png
            fig_batch_speed_Bias.png
```

---

## Main Output Tables

### `run_summary.csv`

Run-level summary table. This is one of the most important outputs.

For non-GT runs, it includes values such as:

- number of tracks
- number of samples
- scale
- fps
- mean/median/std diameter
- mean/median/std velocity
- mean track length
- centroid uncertainty
- diameter uncertainty
- bubble density
- Grace diagram values when available

For GT runs, it includes validation metrics such as:

- number of matched samples
- diameter MAE
- diameter RMSE
- diameter bias
- diameter standard deviation
- speed MAE, RMSE, bias, and standard deviation when GT speed exists
- IoU mean and median
- MOTA
- MOTP
- IDF1
- FP, FN, and ID switches
- KS two-sample test results
- bubble density values

### `track_stats.csv`

Contains track length information for each bubble track.

Typical contents:

- track ID
- track length in frames

This table is useful for checking whether the tracker is maintaining bubbles long enough to produce meaningful velocity and trajectory statistics.

### `per_bubble_stats.csv`

Contains per-track statistics for measured bubble properties.

This can include:

- number of samples per track
- mean diameter per track
- median diameter per track
- min/max diameter per track
- standard deviation of diameter per track
- mean velocity per track
- standard deviation of velocity per track
- centroid statistics

This table is useful for checking variation along individual bubble tracks.

### `per_track_mean_diam_vel.csv`

Contains one row per track with:

- track ID
- number of samples
- mean diameter
- mean velocity

This table supports the per-track mean velocity vs. mean diameter plot.

### `bubble_density_summary.csv`

Summarizes the number of detected bubbles per frame and the approximate bubble density in the observation region.

Typical values include:

- ROI area
- mean bubbles per frame
- median bubbles per frame
- mean bubble density
- median bubble density
- number of frames used

Bubble density is calculated from detections per frame divided by the estimated region-of-interest area.

### `bubble_density_per_frame.csv`

Contains frame-by-frame bubble count and density values.

This table is useful for checking how the number of detected bubbles changes during a run.

### `centroid_uncertainty_summary.csv`

Contains the centroid uncertainty estimate calculated from residuals between measured centroid positions and a smooth fitted trajectory.

This uncertainty estimate is based on frame-to-frame centroid repeatability along tracks.

### `centroid_uncertainty_per_track.csv`

Contains centroid uncertainty values for each track.

### `diameter_uncertainty_summary.csv`

Contains a diameter repeatability uncertainty estimate based on diameter variation along individual tracks.

This is not a GT accuracy measurement. It is a precision/repeatability estimate.

### `diameter_uncertainty_per_track_px.csv`

Contains diameter uncertainty by track in pixels.

If scale is available, the notebook also saves:

```text
diameter_uncertainty_per_track_in.csv
```

### `delta_r_summary.csv`

Summarizes frame-to-frame centroid displacement.

This includes:

- median displacement
- mean displacement
- number of frame-to-frame steps used

This is a motion/track-continuity diagnostic and is not directly used as the main uncertainty value.

---

## GT-Specific Output Tables

### `matched_pairs.csv`

Contains the matched GT and measured bubble pairs.

This table is created after frame-by-frame matching.

Typical columns include:

- frame
- match distance
- GT centroid
- measured centroid
- GT diameter
- measured diameter
- diameter error
- GT speed, when available
- measured speed
- speed error, when available

This is the source table for most GT validation plots.

### `tracking_metrics.csv`

Contains tracking performance metrics.

| Metric | Meaning |
|---|---|
| MOTA | Multiple Object Tracking Accuracy |
| MOTP | Multiple Object Tracking Precision, based on match distance |
| IDF1 | Identity F1 score |
| FP | False positives |
| FN | False negatives |
| IDSW | ID switches |
| GT_total | Total GT objects |
| matches | Number of matched objects |

These metrics are mainly useful for synthetic validation and tracking-quality assessment.

### `iou_per_frame.csv`

Contains semantic segmentation IoU for each video frame.

IoU is calculated from the predicted binary mask video and the GT binary mask video.

Required files:

```text
RunName_pred_mask.mp4
RunName_gt_mask.mp4
```

### `ks2_results.csv`

Contains Kolmogorov-Smirnov two-sample test results comparing GT and measured distributions.

The test is performed for:

- diameter
- speed, when GT speed data exist

This table helps determine whether the measured and GT distributions differ statistically.

### `standardize_summary.csv`

Contains mean and standard deviation values for GT and measured distributions.

This table is used for standardized comparison of measured and true values.

### `standardize_values_diam.csv`

Contains standardized GT and measured diameter values.

### `standardize_values_speed.csv`

Contains standardized GT and measured speed values, when speed data are available.

---

## Main Output Figures

### Diameter distribution

```text
fig_diameter_hist_*.png
```

Shows the measured equivalent bubble diameter distribution.

This figure is useful for describing the size range of bubbles detected in a run.

### Velocity distribution

```text
fig_velocity_hist_*.png
```

Shows the measured bubble velocity distribution.

This figure is useful for describing the velocity range of tracked bubbles.

### Track length distribution

```text
fig_track_lengths.png
```

Shows track length in frames and, when fps is available, track duration in seconds.

This figure helps evaluate whether tracks are long enough to produce meaningful trajectory and velocity statistics.

### Representative trajectories

```text
fig_trajectories_2d_timecolored.png
```

Shows selected representative bubble trajectories colored by time.

This plot is for visualization and diagnostic purposes. It does not show every valid track. Tracks are selected to keep the figure readable.

The plot can be used to check:

- whether trajectories appear physically plausible
- whether track paths are smooth
- whether tracks are being broken unexpectedly
- whether the coordinate system and orientation look correct

For newer experiments where bubbles may move sideways, the plot does not require upward motion.

### Velocity vs. diameter

```text
fig_velocity_vs_diameter_*.png
```

Shows instantaneous velocity as a function of equivalent diameter.

This figure is useful for evaluating whether larger bubbles generally move faster or whether the data show high scatter.

### Mean velocity vs. mean diameter per track

```text
fig_mean_vel_vs_mean_diam_per_track_*.png
```

Shows one point per bubble track.

Each point represents:

- mean diameter for one track
- mean velocity for one track

This plot is often cleaner than the instantaneous velocity-vs-diameter plot because it reduces frame-level scatter.

### Velocity vs. time

```text
fig_velocity_vs_time_*.png
```

Shows velocity history for selected bubble tracks.

This is primarily a diagnostic figure and can be cluttered if many tracks are shown.

### Smoothed velocity vs. time

```text
fig_velocity_vs_time_smoothed_*.png
```

Shows smoothed velocity histories for selected tracks.

The smoothed plot is intended for visualization only. It uses rolling median despiking and rolling mean smoothing to make track behavior easier to interpret.

The plotted tracks are aligned so that each track starts at time zero. This makes it easier to compare how velocity changes over the lifetime of different tracks.

The smoothed values should not replace the raw tracked CSV values for quantitative statistics.

### Velocity smoothness

```text
fig_velocity_smoothness_*.png
```

Shows the distribution of frame-to-frame velocity changes.

This plot is used as a diagnostic for velocity noise, tracking jumps, or unstable measurements.

### Delta-r histogram

```text
fig_delta_r_histogram.png
```

Shows frame-to-frame centroid displacement.

This plot is useful for diagnosing whether tracked bubbles move smoothly or whether there are large jumps that may indicate ID switches or tracking errors.

### Grace diagram point

```text
fig_grace_avg_point.png
```

Shows the run-averaged or median-based Eötvös/Reynolds point on a Grace diagram.

This plot is used to relate measured bubble behavior to expected bubble shape and flow regimes.

### Qualitative sequence

```text
fig_qualitative_sequence.png
```

Shows a short sequence of video frames.

For non-GT runs, this generally includes:

- raw frame
- predicted mask
- filtered/annotated tracking output

For GT runs, this can also include:

- GT mask

This figure is useful for visually checking segmentation and tracking quality.

---

## GT Validation Figures

### Measured vs. true diameter

```text
fig_measured_vs_true_diameter_*.png
```

Shows measured bubble diameter compared to true GT diameter.

The 1:1 line represents perfect agreement.

The stat box reports:

- number of matched samples
- MAE
- RMSE
- bias

This is one of the most important GT validation figures.

### Measured vs. true speed

```text
fig_measured_vs_true_speed_*.png
```

Shows measured speed compared to true GT speed.

This plot is generated only when GT speed data are available.

The 1:1 line represents perfect agreement.

The stat box reports:

- number of matched samples
- MAE
- RMSE
- bias

### Diameter error distribution

```text
fig_error_hist_diameter_*.png
```

Shows the distribution of signed diameter error:

```text
measured diameter - true diameter
```

This figure is useful for checking bias and spread in diameter measurement.

### Speed error distribution

```text
fig_error_hist_speed_*.png
```

Shows the distribution of signed speed error:

```text
measured speed - true speed
```

This plot is generated only when GT speed data are available.

### Relative diameter error

```text
fig_relative_error_diameter.png
```

Shows relative diameter error as a percentage.

This plot can be useful but should be interpreted carefully because relative error can appear large when the true diameter is small.

### Diameter error vs. time

```text
fig_error_vs_time_diameter_*.png
```

Shows diameter error as a function of time or frame number.

This helps identify whether error changes during the run.

### Speed error vs. time

```text
fig_error_vs_time_speed_*.png
```

Shows speed error as a function of time or frame number.

This plot is generated only when GT speed data are available.

### IoU vs. time

```text
fig_iou_vs_time.png
```

Shows semantic segmentation IoU over time.

This is useful for identifying whether segmentation quality changes during a video.

### IoU histogram

```text
fig_iou_histogram.png
```

Shows the distribution of frame-level IoU values.

This is useful for summarizing segmentation quality over the full video.

### Mask difference sequence

```text
fig_mask_diff_sequence.png
```

Shows GT and predicted mask differences for selected frames.

The color convention is:

- white: GT region, including overlap
- red: over-prediction / false positive region
- blue: under-prediction / false negative region

This figure is useful for quickly identifying whether the model tends to over-segment or under-segment bubbles.

---

## Batch Outputs

Batch mode processes all files matching:

```text
*_tracked.csv
```

in the selected batch folder.

### `master_summary.csv`

Contains one row per processed run.

This table is useful for comparing cases and generating batch-level plots.

### Batch diameter overlay

```text
batch_overlays/fig_batch_overlay_diameter.png
```

Overlays diameter distributions from multiple runs.

### Batch velocity overlay

```text
batch_overlays/fig_batch_overlay_velocity.png
```

Overlays velocity distributions from multiple runs.

### Batch Grace diagram

```text
batch_overlays/fig_grace_all_runs_avg.png
```

Shows one Eötvös/Reynolds point per run on the Grace diagram.

### Batch GT accuracy plots

These are saved in:

```text
batch_overlays/accuracy/
```

Possible figures include:

```text
fig_batch_diam_MAE.png
fig_batch_diam_RMSE.png
fig_batch_diam_Bias.png
fig_batch_speed_MAE.png
fig_batch_speed_RMSE.png
fig_batch_speed_Bias.png
```

These plots are generated only when GT cases are present in `master_summary.csv`.

---

## Notes on Quantitative vs. Visualization Figures

Not every figure should be interpreted the same way.

### Strong quantitative figures

These are most appropriate for reporting measured performance:

- measured vs. true diameter
- measured vs. true speed
- diameter error distribution
- speed error distribution
- IoU histogram
- batch MAE/RMSE/bias plots
- run summary tables
- matched pairs table

### Diagnostic or visualization figures

These are mainly for checking behavior and communicating qualitative results:

- representative trajectories
- smoothed velocity vs. time
- qualitative sequence
- mask difference sequence
- velocity smoothness
- delta-r histogram
- track-length distribution

The trajectory and smoothed velocity plots intentionally show selected tracks for clarity. They are not intended to display every valid track.

---

## Common Issues and Fixes

### The trajectory plot says “No valid tracks”

This usually means the display filter is too strict for the dataset.

Common causes include:

- `min_points` is too high
- the time gap threshold is too strict
- the jump threshold is too strict
- low-frame-rate videos have larger frame-to-frame time steps

For synthetic videos at approximately 30 fps, the frame interval is about:

```text
0.033 s
```

So the trajectory display gap limit should be greater than this value.

A useful setting is:

```python
gap_max_s=0.05
```

For sideways or spinning experiments, upward motion should not be required:

```python
require_upward_motion=False
```

### Speed GT plots are skipped

This happens when the GT file does not contain valid speed data.

This is expected for some hand-labeled GT datasets.

Diameter validation and mask validation can still run.

### IoU plots are missing

IoU requires both:

```text
RunName_gt_mask.mp4
RunName_pred_mask.mp4
```

If either video is missing, IoU validation is skipped.

### Physical units are missing

If the notebook cannot find valid scale information, it keeps the output in pixels and pixels per second.

To get inches and inches per second, check that `RunName_averages.csv` contains valid scale information.

### Plots are saved but an error occurred

If a plot fails, the notebook writes a placeholder PNG and logs the error in:

```text
errors.log
```

Check this file first when a figure does not look right or contains a failure message.

---

## Recommended Use

For a typical experimental run without GT:

1. Run the main Bubble Tracking GUI.
2. Confirm that `RunName_tracked.csv` and `RunName_averages.csv` were generated.
3. Open `Results.ipynb`.
4. Run the GUI.
5. Select the tracked CSV or batch folder.
6. Select an output root folder.
7. Run single-file or batch analysis.
8. Review `run_summary.csv`, main figures, and `errors.log`.

For a synthetic or GT validation run:

1. Confirm that `RunName_tracked.csv` exists.
2. Confirm that `RunName_gt.csv` exists.
3. If doing IoU validation, confirm that `RunName_gt_mask.mp4` and `RunName_pred_mask.mp4` exist.
4. Run the results GUI.
5. Select the tracked CSV or batch folder.
6. Select an output root folder.
7. Set the GT match distance.
8. Run the analysis.
9. Review `matched_pairs.csv`, GT validation plots, IoU outputs, and tracking metrics.

---

## Summary

`Results.ipynb` converts raw tracking outputs into organized figures and tables for thesis, journal, and experiment-comparison use. It supports both experimental cases without ground truth and synthetic or hand-labeled validation cases with ground truth. The notebook is designed to produce readable output figures, preserve quantitative values in CSV files, and provide diagnostic plots that help evaluate segmentation, tracking, and measurement quality.
