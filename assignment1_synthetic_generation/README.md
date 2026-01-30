# Assignment 1: Synthetic Dataset Generation

This module generates synthetic plot images with error bars and corresponding JSON annotations.

**Generated Dataset Link:** https://drive.google.com/drive/folders/1PWVcDS5yuk7cJvEYZxY6gWLRvClu2eCK?usp=sharing

## Overview

The generator creates realistic scientific plots using matplotlib with the following features:
- Multiple line series per plot (1-5 lines)
- Configurable error bar styles and sizes
- Randomized visual elements (colors, markers, line styles)
- Optional grid, legend, titles, and axis labels
- Background noise and paper tinting for realism

## Usage

### Running the Generator

Open and run the Jupyter notebook:

```bash
jupyter notebook dataset-generator.ipynb
```

### Configuration

Modify the `SynthConfig` dataclass in the notebook:

```python
@dataclass
class SynthConfig:
    out_root: str = "dataset"
    n_images: int = 3000           # Number of images to generate
    img_w: int = 960               # Image width
    img_h: int = 720               # Image height
    
    n_lines_range: Tuple[int, int] = (1, 5)      # Lines per plot
    n_points_range: Tuple[int, int] = (4, 12)    # Points per line
    
    grid_prob: float = 0.55        # Probability of showing grid
    legend_prob: float = 0.80      # Probability of showing legend
    # ... more parameters
```

### Output

The generator creates:

```
dataset/
├── images/
│   ├── <uuid-1>.png
│   ├── <uuid-2>.png
│   └── ...
└── labels/
    ├── <uuid-1>.json
    ├── <uuid-2>.json
    └── ...
```

Each JSON file contains pixel coordinates and error bar distances for all data points in the corresponding image.

## Key Features

### Plot Variations

- **Line styles**: Solid, dashed, dotted, dash-dot
- **Markers**: Circle, square, triangle, diamond, plus, cross, star
- **Y-axis scales**: Linear and logarithmic
- **X-axis modes**: Linear spacing and irregular spacing
- **Colors**: Randomized from matplotlib colormaps

### Error Bar Generation

- Error bars scaled proportionally to y-values
- Asymmetric upper/lower bars with random variation
- Configurable error size range (3-20% of data value)
- T-bar caps with configurable sizes

### Realism Enhancements

- Optional background noise (35% probability)
- Paper tinting for scanned-document appearance (25% probability)
- Grid lines (55% probability)
- Legend placement (80% probability)
- Axis labels and titles

### Anchor Points

Each line includes four anchor points for axis boundaries:
- xmin, xmax (minimum and maximum x values)
- ymin, ymax (minimum and maximum y values)

These have zero error bar distances and can be filtered using `label != ""`.

## Label Format

Each JSON file contains an array of line data:

```json
[
  {
    "label": {"lineName": "Treatment A"},
    "points": [
      {
        "x": 120.5,
        "y": 340.2,
        "label": "",
        "topBarPixelDistance": 45.0,
        "bottomBarPixelDistance": 50.0,
        "deviationPixelDistance": 2.5
      }
    ]
  }
]
```

**Important distances:**
- `topBarPixelDistance = y_point - y_top_endpoint` (positive when bar goes up)
- `bottomBarPixelDistance = y_bottom_endpoint - y_point` (positive when bar goes down)

## Performance

Generating 3000 images takes approximately 20-30 minutes on a standard machine with progress bars showing real-time status.

## Dependencies

See `requirements.txt` for the complete list.
