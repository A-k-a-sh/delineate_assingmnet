# Assignment 2: Computer Vision Approach

Error bar detection using classical computer vision techniques with OpenCV.

## Overview

This approach uses edge detection, morphological operations, and connected components analysis to detect error bar stems without machine learning.

### Key Algorithm

1. **Preprocessing**
   - Grid removal (morphological opening)
   - Bilateral filtering (denoise while preserving edges)
   - Adaptive thresholding
   - Canny edge detection
   - Vertical dilation to strengthen stems

2. **Primary Detection: Connected Components**
   - Adaptive thresholding for ink extraction
   - Vertical morphological closing (1x11 kernel) to connect stems
   - Filter components by shape (tall, narrow, vertical)
   - Must intersect data point row with ink present
   - Score by height minus 2x horizontal distance

3. **Fallback: Edge Walking**
   - Walk vertically from data point
   - Count edge pixels in horizontal ROI
   - Stop after consecutive rows without stem presence

4. **Refinement**
   - Cap detection for T-bar endpoints
   - X-coordinate optimization for stem centering

## Performance

Evaluated on real dataset:
- Within 5px: ~38-42%
- Within 10px: ~60%

## Usage

### Running Detection

Open and run the Jupyter notebook:

```bash
jupyter notebook error_bar_detection_cv.ipynb
```

### Configuration

Modify the `DetConfig` dataclass in the notebook:

```python
@dataclass
class DetConfig:
    half_w: int = 6               # ROI width around x
    max_up: int = 280             # Vertical search window up
    max_down: int = 280           # Vertical search window down
    stem_count_thr: int = 2       # Stem presence threshold
    cap_band: int = 6             # Cap search range
    # ... more parameters
```

### Dataset Paths

Update these paths in the notebook before running:

```python
base_path_synthetic = "/path/to/synthetic/dataset"
base_path_real = "/path/to/real/dataset"
```

### Quick Test

The notebook includes a single-image test section (Section 11) for quick validation before batch processing.

### Batch Evaluation

Sections 12-13 provide evaluation on synthetic and real datasets with progress bars and detailed metrics.

## Output Format

```json
{
  "image_file": "uuid.png",
  "error_bars": [
    {
      "lineName": "Line 1",
      "points": [
        {
          "data_point": {"x": 120.5, "y": 340.2},
          "upper_error_bar": {"x": 120.5, "y": 295.2},
          "lower_error_bar": {"x": 120.5, "y": 390.2},
          "confidence": 0.85
        }
      ]
    }
  ]
}
```

## Key Features

- No training required
- Fast inference (milliseconds per image)
- Interpretable algorithm with explicit geometric reasoning
- Graceful fallbacks for difficult cases
- Confidence scores based on stem strength and geometry

## Limitations

- Performance degrades with thick/overlapping error bars
- Requires clean edges for reliable detection
- Background noise can interfere with edge detection
- Fixed threshold parameters may not generalize to all plot styles

## Dependencies

See `requirements.txt` for the complete list.
