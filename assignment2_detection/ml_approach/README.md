# Assignment 2: Machine Learning Approach

Error bar detection using deep learning with CNN regression on image crops.

## Overview

This approach uses a ResNet-based convolutional neural network to regress error bar offsets from cropped regions around data points.

### Architecture

- **Backbone:** ResNet18/34 (pretrained on ImageNet)
- **Head:** Fully connected layers with dropout
- **Output:** 2 values (normalized dy_up, dy_down offsets)
- **Input:** 128x384 crops centered on data points

### Training Strategy

1. **Pretraining:** Train on synthetic dataset (2707 images, 68k+ points)
2. **Fine-tuning:** Fine-tune on real dataset (120 train, 30 test)
3. **Normalization:** Offsets normalized by max_offset=280.0
4. **Loss:** MSE loss on normalized offsets

## Performance

Evaluated on real dataset:
- Within 5px: ~30%
- Within 10px: ~66%

Note: CV approach currently outperforms ML on real data, likely due to limited real training data.

## Usage

### Running Detection

Open and run the Jupyter notebook:

```bash
jupyter notebook error_bar_detection_ml.ipynb
```

### Configuration

Modify the `Config` dataclass in the notebook:

```python
@dataclass
class Config:
    crop_width: int = 128
    crop_height: int = 384
    max_offset: float = 280.0
    backbone: str = "resnet18"     # or "resnet34"
    batch_size: int = 32
    num_epochs_pretrain: int = 5
    num_epochs_finetune: int = 20
    # ... more parameters
```

### Dataset Paths

Update these paths in the notebook before running:

```python
synthetic_images: str = "/path/to/synthetic/images"
synthetic_labels: str = "/path/to/synthetic/labels"
real_images: str = "/path/to/real/images"
real_labels: str = "/path/to/real/labels"
```

### Training Pipeline

The notebook includes:
1. Dataset loading and preprocessing
2. Model architecture definition
3. Training loop with validation
4. Fine-tuning on real data
5. Evaluation and visualization

### GPU Support

The code automatically detects and uses CUDA if available. For CPU-only execution, it will fall back gracefully.

## Output Format

Same as CV approach:

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

- Transfer learning from ImageNet
- Data augmentation (brightness, contrast, noise)
- End-to-end learnable pipeline
- Confidence estimation from prediction variance

## Limitations

- Requires GPU for efficient training
- Performance limited by small real dataset size
- Less interpretable than CV approach
- Longer inference time compared to CV

## Future Improvements

- Collect more labeled real data
- Experiment with attention mechanisms
- Multi-task learning (bar presence + endpoints)
- Ensemble with CV approach

## Dependencies

See `requirements.txt` for the complete list.
