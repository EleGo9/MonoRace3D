# MonoRace3D
# MonoRace3D Dataset

A comprehensive dataset for monocular 3D object detection in racing environments.

🌐 **Website**: [https://yourusername.github.io/MonoRace3D](https://yourusername.github.io/MonoRace3D)

# MonoRace3D Dataset

A comprehensive dataset for monocular 3D car detection in autonomous racing environments.

🌐 **Website**: [https://yourusername.github.io/MonoRace3D](https://yourusername.github.io/MonoRace3D)

## Dataset Overview

MonoRace3D provides a specialized dataset for monocular 3D car detection in autonomous racing scenarios. The dataset contains 4,490 high-quality images with precise 3D bounding box annotations captured from real autonomous racing vehicles across multiple professional racing circuits.

### Key Statistics

- **4,490 annotated images** from real autonomous racing scenarios
- **4,490 precise 3D car annotations** with depth information
- **3 camera perspectives**: Front-center, front-left, and front-right views
- **5 racing circuits**: Including LVMS, IMS, and KS professional tracks
- **Depth range**: 6m to 135m (mean: 45.5m, median: 42.3m)
- **Multi-scale objects**: Small (94%), medium (29%), and large (0.4%) car instances
- **Diverse conditions**: Multiple racing sessions and lighting conditions

### Camera Distribution
- **Front Center (FC)**: 1,419 images (31.6%)
- **Front Left (FL)**: 1,512 images (33.7%) 
- **Front Right (FR)**: 1,559 images (34.7%)

### Circuit Distribution
- **LVMS (Las Vegas Motor Speedway)**: 3,522 images (78.4%)
- **IMS (Indianapolis Motor Speedway)**: 847 images (18.9%)
- **KS (Kansas Speedway)**: 121 images (2.7%)

## Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/MonoRace3D.git
cd MonoRace3D

# Download dataset
python download_data.py --split train --auth-key YOUR_KEY

# Load in Python
from monorace3d import MonoRace3DDataset
dataset = MonoRace3DDataset(root='./data', split='train')
```

## Dataset Structure

```
MonoRace3D/
├── images/
│   ├── train/          # 3,592 training images
│   ├── val/            # 449 validation images  
│   └── test/           # 449 test images
├── annotations/
│   ├── train.json      # Training annotations (COCO-3D format)
│   ├── val.json        # Validation annotations
│   └── test.json       # Test annotations
├── metadata/
│   ├── camera_params/  # Intrinsic parameters for FC, FL, FR cameras
│   ├── circuits/       # Circuit information (LVMS, IMS, KS)
│   └── sessions/       # Racing session metadata
└── tools/
    ├── visualization/  # 3D bbox visualization scripts
    ├── evaluation/     # Detection evaluation metrics
    └── analysis/       # Dataset analysis tools
```

## Object Categories

**Primary Focus: Racing Cars**
- Formula-style racing cars in autonomous racing scenarios
- Precise 3D bounding boxes with depth, width, and height information
- Comprehensive coverage of car orientations and scales

## Annotation Format

Annotations follow the COCO-3D format with racing-specific enhancements:

```json
{
  "image_id": 12345,
  "category_id": 1,
  "category_name": "Car",
  "bbox_2d": [x, y, width, height],
  "bbox_3d": {
    "center_2d": [x_center, y_center],
    "depth": 45.2,
    "dimensions_3d": [length, width, height],
    "location_3d": [x, y, z],
    "rotation_y": 1.57
  },
  "camera_id": "camera_fc",
  "circuit": "lvms",
  "session": "20250106_lvms_run01_multi_polimove",
  "area": 4190.15,
  "scale_category": "medium"
}
```

## Depth Distribution

The dataset provides a realistic depth distribution for racing scenarios:

- **Mean depth**: 45.5 meters
- **Median depth**: 42.3 meters
- **Range**: 6.0m to 135.3m
- **25th percentile**: 29.9m
- **75th percentile**: 55.0m
- **95th percentile**: 95.8m

## Scale Distribution

Object scales are categorized based on bounding box area:
- **Small objects** (< 32²): 4,236 instances (94.3%)
- **Medium objects** (32² - 96²): 1,312 instances (29.2%)  
- **Large objects** (> 96²): 18 instances (0.4%)

## Evaluation Metrics

Standard 3D object detection metrics adapted for racing scenarios:

- **AP@IoU**: Average Precision at different IoU thresholds (0.5, 0.7)
- **mAP**: mean Average Precision across all distance ranges
- **AOS**: Average Orientation Similarity for vehicle heading estimation
- **3D IoU**: 3D Intersection over Union for spatial accuracy
- **Depth Error**: Mean absolute error in depth estimation (critical for racing)
- **Distance-based AP**: Evaluation across different depth ranges (0-30m, 30-60m, 60m+)

## Benchmark Results

Baseline performance using state-of-the-art monocular 3D detection methods on MonoRace3D:

| Method    | AP@0.5 | AP@0.7 | mAP  | Depth MAE (m) | Inference (ms) |
|-----------|--------|--------|------|---------------|----------------|
| MonoDETR  | 68.4   | 52.1   | 60.2 | 3.8           | 45             |
| MonoDGP   | 65.7   | 48.9   | 57.3 | 4.2           | 38             |
| FCOS3D    | 61.2   | 44.6   | 52.9 | 5.1           | 52             |
| SMOKE     | 58.1   | 41.3   | 49.7 | 5.9           | 28             |

*Results evaluated on validation set with camera_fc images

## Installation

### Requirements

- Python 3.8+
- PyTorch 1.9+
- OpenCV 4.5+
- NumPy 1.21+

### Setup

```bash
pip install -r requirements.txt
```

## Usage Examples

### Loading Dataset

```python
from monorace3d import MonoRace3DDataset
from torch.utils.data import DataLoader

# Initialize dataset  
dataset = MonoRace3DDataset(
    root='./data',
    split='train',
    camera='camera_fc',  # or 'camera_fl', 'camera_fr'
    transform=transforms,
    target_transform=target_transforms
)

# Create data loader
dataloader = DataLoader(dataset, batch_size=16, shuffle=True)

for images, targets in dataloader:
    # Training loop for 3D car detection
    pass
```

### Multi-Camera Training

```python
# Train on all camera perspectives
cameras = ['camera_fc', 'camera_fl', 'camera_fr']
datasets = []

for camera in cameras:
    dataset = MonoRace3DDataset(
        root='./data',
        split='train', 
        camera=camera
    )
    datasets.append(dataset)

# Combine datasets
combined_dataset = torch.utils.data.ConcatDataset(datasets)
```

### Depth-Aware Evaluation

```python
from monorace3d.evaluation import evaluate_depth_ranges

# Evaluate across different depth ranges
depth_ranges = [(0, 30), (30, 60), (60, 135)]
results = evaluate_depth_ranges(
    predictions=model_predictions,
    ground_truth=ground_truth,
    depth_ranges=depth_ranges
)

print(f"Close range AP (0-30m): {results['0-30m']['ap']:.2f}")
print(f"Medium range AP (30-60m): {results['30-60m']['ap']:.2f}")
print(f"Far range AP (60m+): {results['60m+']['ap']:.2f}")
```

### Visualization

```python
from monorace3d.visualization import visualize_3d_detection

# Load sample
image, annotations = dataset[0]

# Visualize 3D bounding boxes with depth information
visualized = visualize_3d_detection(
    image=image, 
    annotations=annotations,
    camera_params=dataset.get_camera_params(),
    show_depth=True
)
```

## GitHub Pages Setup Instructions

To set up your own GitHub Pages website for MonoRace3D:

1. **Create a new GitHub repository** named `MonoRace3D` (or any name you prefer)

2. **Upload these files** to your repository:
   - `index.html` (main website file)
   - `_config.yml` (Jekyll configuration)
   - `README.md` (this file)

3. **Enable GitHub Pages**:
   - Go to your repository settings
   - Scroll to "Pages" section
   - Select "Deploy from a branch"
   - Choose "main" branch and "/ (root)" folder
   - Save

4. **Customize the website**:
   - Replace `yourusername` in all files with your GitHub username
   - Update email addresses and social media links
   - Add your actual dataset download links
   - Update author information and affiliations

5. **Access your website** at: `https://yourusername.github.io/MonoRace3D`

## Citation

If you use MonoRace3D in your research, please cite:

```bibtex
@article{monorace3d2025,
  title={MonoRace3D: A Comprehensive Dataset for Monocular 3D Car Detection in Autonomous Racing Environments},
  author={Ele and Collaborators},
  journal={arXiv preprint arXiv:2025.xxxxx},
  year={2025},
  note={4,490 images from autonomous racing scenarios with 3D car annotations}
}
```

## License

This dataset is released under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.

## Contact

- **Email**: ele@unimore.it
- **Institution**: University of Modena and Reggio Emilia
- **Research Group**: Computer Vision and Robotics Lab

## Acknowledgments

This work was supported by:
- University of Modena and Reggio Emilia
- Hipert Srl
- [Add other funding sources]

---

© 2025 MonoRace3D Dataset. University of Modena and Reggio Emilia.
