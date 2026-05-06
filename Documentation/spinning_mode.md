# Spinning Mode

## Overview

Spinning mode is an experimental analysis mode developed for rotating-apparatus bubble experiments.

This functionality extends beyond the primary thesis methodology and was added after the original thesis work was completed.

The mode was designed to improve bubble tracking and timing accuracy in experiments where:

- the apparatus rotates during acquisition
- frame spacing is nonuniform
- the camera orientation alternates between bursts
- apparatus motion causes image translation between frames

---

## Main Features

Spinning mode includes:

- OCR/template-based timestamp extraction
- burst-based processing
- optional frame flipping
- apparatus stabilization
- rotation-aware timing
- effective radial acceleration calculations

---

## Timestamp Extraction

Frame timing is determined using timestamp digits embedded within the video frames.

The software:

1. Crops the timestamp region.
2. Converts the image to grayscale.
3. Applies thresholding.
4. Matches digit templates using OpenCV template matching.
5. Reconstructs timestamps frame-by-frame.

Timing is therefore based on extracted timestamps rather than a fixed FPS assumption.

This is intended to improve timing accuracy when:

- frame spacing varies
- acquisition timing is irregular
- burst capture modes are used

---

## Burst-Based Processing

Spinning data is processed in bursts.

The user specifies:

- frames per burst
- whether tracks should reset between bursts

Burst handling allows the software to account for:

- repeated rotational passes
- periodic camera orientation changes
- intermittent acquisition

---

## Alternating Frame Orientation

In some spinning configurations, alternating bursts may appear horizontally flipped due to apparatus rotation.

The software can compensate for this by:

- identifying odd-numbered bursts
- horizontally flipping frames before processing

This produces a consistent tracking orientation across the dataset.

---

## Apparatus Stabilization

Additional stabilization logic was added for spinning-mode experiments.

The software identifies the midpoint of the top-center apparatus bar and uses it as a reference location.

For each frame:

1. The bar midpoint is detected.
2. Horizontal displacement is computed.
3. An affine translation is applied.
4. The apparatus is re-centered prior to detection and tracking.

This reduces apparent apparatus motion between frames and is intended to improve tracking consistency.

---

## Effective Acceleration

When RPM is provided, spinning mode computes an effective radial acceleration using:

```text
g_eff = omega^2 * r
```

where:

- `omega` is angular velocity
- `r` is apparatus radius

The effective acceleration is used when computing dimensionless parameters.

---

## Current Validation Status

Spinning mode is currently implemented as an experimental software feature.

At the time of repository organization:

- still-mode processing has been used for the primary thesis analysis
- spinning-mode code has been implemented
- full validation on spinning experimental datasets has not yet been completed

Future spinning-mode validation should compare:

- timestamp extraction accuracy
- apparatus centering stability
- track continuity
- measured bubble velocity consistency
- dimensionless parameter outputs under rotating conditions

---

## Current Limitations

Spinning mode has several limitations:

- timestamp extraction depends on image quality
- template matching may fail under poor contrast or motion blur
- alignment assumes visibility of the top-center apparatus bar
- burst timing assumptions may not apply to all acquisition modes

---

## Notes

Spinning mode functionality is not fully documented in the thesis manuscript.

The implementation is included in this repository as an experimental extension to the primary Bubble Evaluation Tool framework.