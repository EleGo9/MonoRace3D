# MonoRace3D Dataset

A comprehensive dataset for monocular 3D object detection in racing setting.

## Dataset Overview

MonoRace3D provides a specialized dataset for monocular 3D car detection in autonomous racing scenarios. The dataset contains 4,490 images for training 584 for test, and  with precise 3D bounding box annotations captured from real autonomous racing vehicles across multiple professional racing circuits.

### Key Statistics

- **4,490 + 584 images** from real autonomous racing scenarios
- **4,490 + 584 3D car annotations** with depth information
- **3 camera perspectives**: Front-center, front-left, and front-right views
- **3 racing circuits**: Las Vegas Motor Speedway, Kentucky Speedway, Indianapolis Motor Speedway.
- **Depth range**: 6m to 135m (mean: 45.5m, median: 42.3m)
- **Multi-scale objects**: Small (94%), medium (29%), and large (0.4%) car instances
- **Diverse conditions**: Multiple racing sessions and lighting conditions

### Camera Distribution
- **Front Center (FC)**: 1,419 images (31.6%)
- **Front Left (FL)**: 1,512 images (33.7%) 
- **Front Right (FR)**: 1,559 images (34.7%)

<!-- ### Circuit Distribution
- **LVMS (Las Vegas Motor Speedway)**: 3,522 images (78.4%)
- **IMS (Indianapolis Motor Speedway)**: 847 images (18.9%)
- **KS (Kansas Speedway)**: 121 images (2.7%) -->

<!-- ## Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/MonoRace3D.git
cd MonoRace3D

# Download dataset
python download_data.py --split train --auth-key YOUR_KEY

# Load in Python
from monorace3d import MonoRace3DDataset
dataset = MonoRace3DDataset(root='./data', split='train') -->
<!-- ``` -->

## Dataset Structure

```
MonoRace3D/
├── training/
│   ├── image_2/            # 4'490 training images
│   ├── label_2/            # 4'490 labels  
│   └── calib/              # 4'490 camera parameters
├── test_fr/
│   ├── image_2/            # 353 training images
│   ├── label_2/            # 353 labels  
│   └── calib/              # 353 camera parameters
├── test_fc/
│   ├── image_2/            # 231 training images
│   ├── label_2/            # 231 labels  
│   └── calib/              # 231 camera parameters
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

- **AP 3D**: Average Precision of 3D Bounding Boxes, threshold set to 0.7;
- **AP BEV**: Average Precision of BEV bounding boxes, threshold set to 0.7;
- **Depth Error**: Mean absolute error in depth estimation (critical for racing), only for valid bounding box, expressed in meters;
- **Rotation Error**: Yaw rotation error of valid bounding boxes, expressed in degrees

## Benchmark Results

Baseline performance using state-of-the-art monocular 3D detection methods on MonoRace3D:

Results evaluated on validation set with camera_fc images

| Method    | APBEV@0.7 | AP3D@0.7 | Depth MAE (m) | Rotation Err(°) |
|-----------|-----------|----------|---------------|-----------------|
| MonoDETR  | 19.95     | 13.19    | 4.17m         | 3.06°           |
| MonoDGP   | 28.14     | 15.17    | 1.25m         | 3.52°           |
| MonoDPT   | 29.51     | 22.02    | 2.25m         | 2.68°           |

Results evaluated on validation set with camera_fr images

| Method    | APBEV@0.7 | AP3D@0.7 | Depth MAE (m) | Rotation Err(°) |
|-----------|--------   |--------  |---------------|-----------------|
| MonoDETR  | 25.75     | 15.88    | 1.32m         | 3.35°           |
| MonoDGP   | 31.13     | 16.80    | 1.25m         | 3.66°           | 
| MonoDPT   | 46.47     | 34.24    | 0.95m         | 2.38°           |