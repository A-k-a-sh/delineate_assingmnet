# Error Bar Detection in Scientific Plots

This repository contains a complete pipeline for detecting error bars in scientific plot images, including synthetic data generation, detection algorithms, and evaluation frameworks.

## Repository Structure

```
.
├── assignment1_synthetic_generation/   # Synthetic dataset generation pipeline
├── assignment2_detection/
│   ├── cv_approach/                    # Computer vision-based detection
│   └── ml_approach/                    # Machine learning-based detection
├── eda/                                # Exploratory data analysis notebooks
├── docs/                               # Technical report and documentation
├── outputs/                            # Detection results and output JSONs
└── visualizations/                     # Sample visualizations and plots
```

## Assignments

### Assignment 1: Synthetic Dataset Generation

Generate 3000 synthetic plot images with corresponding JSON annotations to augment the training dataset.

**Location:** `assignment1_synthetic_generation/`

**Key Features:**
- Matplotlib-based plot generation
- Configurable plot styles, colors, and error bar sizes
- Automatic JSON label generation with pixel-accurate coordinates
- Realistic variations in grid, legend, and background

**Dataset Link:** https://drive.google.com/drive/folders/1PWVcDS5yuk7cJvEYZxY6gWLRvClu2eCK?usp=sharing

See `assignment1_synthetic_generation/README.md` for detailed usage.

### Assignment 2: Error Bar Detection

Detect upper and lower error bar endpoints given plot images and data point coordinates.

**Location:** `assignment2_detection/`

Two approaches implemented:

1. **Computer Vision Approach** (`cv_approach/`)
   - Edge detection with connected components analysis
   - Morphological operations for stem detection
   - Performance: ~40% within 5px, ~60% within 10px on real data

2. **Machine Learning Approach** (`ml_approach/`)
   - ResNet-based CNN regressor
   - Pretrained on synthetic data, fine-tuned on real data
   - Performance: ~30% within 5px, ~66% within 10px on real data

See respective README files for detailed usage and implementation details.

### Assignment 3: Technical Report

Comprehensive technical report covering:
- Synthetic data generation strategy
- Error bar detection approaches
- Evaluation metrics and results
- Limitations and future work

**Report Location:** `docs/Technical report.pdf`

**Public Link:** https://drive.google.com/file/d/11ik0tqWAfmbPlv0WEbkXLSSAa6iv2-qe/view?usp=sharing

## Quick Start

### 1. Generate Synthetic Dataset

```bash
cd assignment1_synthetic_generation
pip install -r requirements.txt
jupyter notebook dataset-generator.ipynb
```

Configure the number of images in the notebook and run all cells.

### 2. Run Error Bar Detection

**Option A: Computer Vision Approach**

```bash
cd assignment2_detection/cv_approach
pip install -r requirements.txt
jupyter notebook error_bar_detection_cv.ipynb
```

**Option B: Machine Learning Approach**

```bash
cd assignment2_detection/ml_approach
pip install -r requirements.txt
jupyter notebook error_bar_detection_ml.ipynb
```

Update dataset paths in the notebooks before running.

### 3. Exploratory Data Analysis

```bash
cd eda
jupyter notebook synthetic_data_eda.ipynb  # or real_data_eda.ipynb
```

## Dataset Format

The dataset follows this structure:

```
dataset/
├── images/
│   ├── <uuid>.png
│   └── ...
└── labels/
    ├── <uuid>.json
    └── ...
```

Each JSON label file contains:

```json
[
  {
    "label": {"lineName": "Line 1"},
    "points": [
      {
        "x": 120.5,
        "y": 340.2,
        "label": "",
        "topBarPixelDistance": 45.0,
        "bottomBarPixelDistance": 50.0,
        "deviationPixelDistance": 0.0
      }
    ]
  }
]
```

**Coordinate System:** Origin (0,0) is at the top-left corner. X increases rightward, Y increases downward.

## Output Format

Detection outputs follow this format:

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

## Requirements

- Python 3.8+
- OpenCV 4.x
- PyTorch 2.x (for ML approach)
- Jupyter Notebook
- NumPy, Matplotlib, Pillow

See individual `requirements.txt` files in each subdirectory for complete dependencies.

## Evaluation Metrics

All approaches are evaluated using:
- Mean/median pixel error for upper and lower endpoints
- Percentage of predictions within 2px, 5px, and 10px thresholds
- Confidence scores for detection quality

## Links

- **Generated Dataset:** https://drive.google.com/drive/folders/1PWVcDS5yuk7cJvEYZxY6gWLRvClu2eCK?usp=sharing
- **Technical Report:** https://drive.google.com/file/d/11ik0tqWAfmbPlv0WEbkXLSSAa6iv2-qe/view?usp=sharing
- **Original Real Dataset:** https://drive.google.com/drive/folders/1ocgNAZtQ3yYstIdES9ZQO1TTSt5vrJex

## License

This project was developed as part of a technical assignment.
