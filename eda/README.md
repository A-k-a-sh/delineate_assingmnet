# Exploratory Data Analysis

This directory contains notebooks for analyzing the synthetic and real datasets.

## Notebooks

### synthetic_data_eda.ipynb

Analysis of the generated synthetic dataset:
- Image and label count statistics
- Distribution of lines per image
- Distribution of points per line
- Error bar size distributions
- Visual characteristics (colors, markers, styles)
- Sample visualizations

### real_data_eda.ipynb

Analysis of the real dataset:
- Dataset structure and format validation
- Error bar distance statistics
- Comparison with synthetic data
- Quality checks and inconsistencies
- Sample visualizations

## Usage

```bash
jupyter notebook synthetic_data_eda.ipynb
# or
jupyter notebook real_data_eda.ipynb
```

Update dataset paths in the notebooks before running.

## Visualizations

Sample output plots are saved in the `../visualizations/` directory.

## Dependencies

Standard data science stack:
- numpy
- matplotlib
- pandas (optional)
- jupyter
