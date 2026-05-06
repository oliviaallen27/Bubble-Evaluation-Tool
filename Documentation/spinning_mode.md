# Spinning Mode

# Overview

Spinning mode is an experimental analysis mode developed for rotating-apparatus bubble experiments.

This functionality extends beyond the primary thesis methodology and was added after the original thesis work was completed.

The mode was designed to improve bubble tracking and timing accuracy in experiments where:
- the apparatus rotates during acquisition
- frame spacing is nonuniform
- the camera orientation alternates between bursts
- apparatus motion causes image translation between frames

---

# Main Features

Spinning mode includes:

- OCR/template-based timestamp extraction
- Burst-based processing
- Optional frame flipping
- Apparatus stabilization
- Rotation-aware timing
- Effective radial acceleration calculations

---

# Timestamp Extraction

Frame timing is determined using timestamp digits embedded within the video frames.

The software:
1. Crops the timestamp region
2. Converts the image to grayscale
3. Applies thresholding
4. Matches digit templates using OpenCV template matching
5. Reconstructs timestamps frame-by-frame

Timing is therefore based on extracted timestamps rather than a fixed FPS assumption.

This improves timing accuracy when:
- frame spacing varies
- acquisition timing is irregular
- burst capture modes are used

---

# Burst-Based Processing

Spinning data is processed in bursts.

The user specifies:
- frames per burst
- whether tracks should reset between bursts

Burst handling allows the software to account for:
- repeated rotational passes
- periodic camera orientation changes
- intermittent acquisition

---

# Alternating Frame Orientation

In some spinning configurations, alternating bursts appear horizontally flipped due to apparatus rotation.

The software compensates for this by:
- identifying odd-numbered bursts
- horizontally flipping frames before processing

This produces a consistent tracking orientation across the dataset.

---

# Apparatus Stabilization

Additional stabilization logic was added for spinning-mode experiments.

The software identifies the midpoint of the top-center apparatus bar and uses it as a reference location.

For each frame:
1. The bar midpoint is detected
2. Horizontal displacement is computed
3. An affine translation is applied
4. The apparatus is re-centered prior to detection and tracking

This reduces apparent apparatus motion between frames and improves tracking consistency.

---

# Effective Acceleration

When RPM is provided, spinning mode computes an effective radial acceleration using:

```
g_eff = ω²r
```

where:
- `ω` is angular velocity
- `r` is apparatus radius

The effective acceleration is used when computing dimensionless parameters.

---

# Current Limitations

Spinning mode remains experimental and has several limitations:

- Timestamp extraction depends on image quality
- Template matching may fail under poor contrast or motion blur
- Alignment assumes visibility of the top-center apparatus bar
- Burst timing assumptions may not apply to all acquisition modes
- Performance has not been validated across all rotating configurations

---

# Notes

Spinning mode functionality is not fully documented in the thesis manuscript.

The implementation is included in this repository as an experimental extension to the primary Bubble Evaluation Tool framework.