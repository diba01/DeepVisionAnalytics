# DeepVisionAnalytics

## Introduction
The "Wait Time Optimization and Analysis of Interactions in Public Areas" project, part of the CTE SQUARE Pesaro, aims to monitor and analyze the flow of people using advanced Computer Vision (CV) and Deep Learning (DL) techniques. By installing webcams in Piazza del Popolo in Pesaro and employing YOLOv8 (You Only Look Once) neural networks, the project seeks to reduce waiting times and better understand people’s behaviors and attention in public spaces.

## Table of Contents
- [Project Overview](#project-overview)
- [Supervision](#supervision)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Results](#results)
- [Architecture modify](#architecture-modifications)
- [Video Processing](#video-processing)
- [Contributions](#contributions)
- [License](#license)

## Project Overview
This project focuses on three main objectives:
1. **Wait Time Analysis**: Monitoring the duration people spend in specific areas to identify bottlenecks.
2. **Model Optimization**: Enhancing DL models for deployment on edge devices.
3. **Practical Implementation**: Installing webcams and deploying models in real-world scenarios.

## Supervision
<div align="center">
  <p>
    <a align="center" href="https://supervision.roboflow.com" target="_blank">
      <img width="100%" src="https://media.roboflow.com/open-source/supervision/rf-supervision-banner.png?updatedAt=1678995927529" alt="Supervision Library">
    </a>
  </p>
</div>

Supervision is a powerful library used to enhance computer vision and deep learning applications. It simplifies the process of training, evaluating, and deploying DL models. Its features include data augmentation, model evaluation metrics, and support for various neural network architectures.

## Features
- **Real-time Wait Time Analysis**
- **Edge Device Optimization**
- **Deployment in Public Areas**
## 💻 installation

####  1. Install the Supervision Package via Pip

To install the supervision package in a [**Python>=3.8**](https://www.python.org/) environment, use the following command:
```bash
pip install supervision
```
#### 2. Verify Installation
To verify that the installation was successful, run the following commands:
```bash
import supervision
print(supervision.__version__)
```
#### 3. Install Miniconda
Download and install Miniconda from the official Miniconda website. [Miniconda website](https://docs.conda.io/en/latest/miniconda.html).

#### 4. Add Conda to Your Environment Variables
Ensure that Conda is added to your environment variables during the installation process.

#### 5. Install the Supervision Package via Conda
Once Miniconda is installed and configured, open your terminal (or Anaconda Prompt on Windows) and run the following command:
If no errors occur and the version number is displayed, the installation was successful.
Then to install the necessary package, please follow these steps:
```bash
conda install -c conda-forge supervision
```
#### 6. Clone the Repository and Set Up the Python Environment
Clone the repository and navigate to the root directory:
```bash
git clone https://github.com/andreaFaccenda00/DeepVisionAnalytics.git
```
Set up the Python environment and activate it:
```bash
python3 -m venv venv
source venv\Scripts\activate
pip install --upgrade pip
```
Perform a headless install:
```bash
pip install -e "."
```
For desktop installation, use:
```bash
pip install -e ".[desktop]"
```

#### 7. Install Required Dependencies
Install the required dependencies listed in requirements.txt:
```bash
pip install -r requirements.txt
```

## 🛠 usage

### `draw_zones`

If you want to test zone time in zone analysis on your own video, you can use this
script to design custom zones and save results as a JSON file. The script will open a
window where you can draw polygons on the source image or video file. The polygons will
be saved as a JSON file.

- `--source_path`: Path to the source image or video file for drawing polygons.
- `--zone_configuration_path`: Path where the polygon annotations will be saved as a JSON file.


- `enter` - finish drawing the current polygon.
- `escape` - cancel drawing the current polygon.
- `q` - quit the drawing window.
- `s` - save zone configuration to a JSON file.

```bash
python scripts/draw_zones.py 
--source_path "data/people.mp4" 
--zone_configuration_path "data/config.json"
```

https://github.com/roboflow/supervision/assets/26109316/9d514c9e-2a61-418b-ae49-6ac1ad6ae5ac
## usage
### YOLOv8 Training for People Flow Analysis

#### YOLOv8 Training
We trained YOLOv8 variants (YOLOv8n, YOLOv8s, YOLOv8m, YOLOv8l) on the MOTSynth and EuroCity Persons datasets. MOTSynth is a synthetic dataset generated using GTA V, while EuroCity Persons consists of urban images from various European cities.

- **Datasets**: 
  - **MOTSynth**: Large-scale synthetic dataset for pedestrian detection, segmentation, and tracking.
  - **EuroCity Persons**: Urban images from multiple European cities for realistic pedestrian detection.

- **Training Split**: 80% for training, 10% for validation, 10% for testing.
- **Image Size**: 640x640 pixels.

#### Hyperparameters for All YOLOv8 Variants
| Hyperparameter       | YOLOv8n  | YOLOv8s  | YOLOv8m  | YOLOv8l  |
|----------------------|----------|----------|----------|----------|
| Batch size           | 16       | 16       | 16       | 16       |
| Image size           | 640x640  | 640x640  | 640x640  | 640x640  |
| Epochs               | 250      | 250      | 250      | 250      |
| Early stopping       | 30       | 30       | 30       | 30       |
| Optimizer            | SGD      | SGD      | SGD      | SGD      |
| Initial learning rate| 0.01     | 0.01     | 0.01     | 0.01     |
| LR reduction factor  | 0.01     | 0.01     | 0.01     | 0.01     |
| Momentum             | 0.95     | 0.95     | 0.95     | 0.95     |
| Weight decay         | 0.0001   | 0.0001   | 0.0001   | 0.0001   |
| IOU threshold        | 0.7      | 0.7      | 0.7      | 0.7      |
| Detection limit      | 300      | 300      | 300      | 300      |
| Mixed precision      | Yes      | Yes      | Yes      | Yes      |
| Warmup epochs        | 10       | 10       | 10       | 10       |
| Warmup momentum      | 0.5      | 0.5      | 0.5      | 0.5      |
| Warmup LR            | 0.1      | 0.1      | 0.1      | 0.1      |
| Masking ratio        | 4        | 4        | 4        | 4        |

#### Testing and Evaluation
The trained YOLOv8 variants were tested on the SOMPT22 dataset in addition to the MOTSynth and EuroCity Persons datasets. SOMPT22 was used exclusively for testing to provide a rigorous evaluation of the model's capability in urban surveillance scenarios.

| Dataset           | Model   | Inference | mAP@50 | mAP@50-95 | Precision | Recall |
|-------------------|---------|-----------|--------|-----------|-----------|--------|
| MOTSynth          | YOLOv8n | 1.4ms     | 0.729  | 0.430     | 0.816     | 0.585  |
|                   | YOLOv8s | 2.1ms     | 0.749  | 0.449     | 0.836     | 0.606  |
|                   | YOLOv8m | 4.5ms     | 0.777  | 0.483     | 0.849     | 0.648  |
|                   | YOLOv8l | 7.6ms     | 0.764  | 0.47      | 0.86      | 0.617  |
| EuroCity Persons  | YOLOv8n | 1.7ms     | 0.602  | 0.401     | 0.721     | 0.287  |
|                   | YOLOv8s | 3.7ms     | 0.629  | 0.439     | 0.846     | 0.298  |
|                   | YOLOv8m | 7.9ms     | 0.633  | 0.415     | 0.844     | 0.388  |
|                   | YOLOv8l | 13.5ms    | 0.621  | 0.425     | 0.900     | 0.328  |
| MOTSynth EuroCity | YOLOv8n | 1.2ms     | 0.741  | 0.454     | 0.824     | 0.600  |
|                   | YOLOv8s | 2.2ms     | 0.778  | 0.485     | 0.850     | 0.651  |
|                   | YOLOv8m | 4.6ms     | 0.781  | 0.488     | 0.854     | 0.652  |
|                   | YOLOv8l | 7.6ms     | 0.786  | 0.487     | 0.859     | 0.659  |
| Coco              | YOLOv8n | 1.2ms     | 0.626  | 0.434     | 0.799     | 0.377  |
|                   | YOLOv8s | 2.2ms     | 0.641  | 0.459     | 0.810     | 0.447  |
|                   | YOLOv8m | 4.6ms     | 0.649  | 0.471     | 0.809     | 0.463  |
|                   | YOLOv8l | 7.9ms     | 0.643  | 0.465     | 0.809     | 0.431  |

YOLOv8s was chosen for its balance of rapid inference (2.4ms), high precision (0.956), and significant recall (0.742), making it suitable for real-time surveillance and monitoring.

All trained models can be accessed via this [OneDrive link][https://univpm-my.sharepoint.com/:f:/g/personal/s1119359_studenti_univpm_it/EsUTP-Jwi65GlAL8wUo6WzIBGzOsCs2ITQL4bhMY-cACqg?e=iM0VQh].

## Results
The following images demonstrate the performance and evaluation metrics of the YOLOv8s model trained on MOTSynth & Eurocity:

- **Confusion Matrix**:
-  <img src="https://github.com/andreaFaccenda00/DeepVisionAnalytics/assets/171338421/5918e13d-4d1f-4c21-bb85-dd2a99a889f0" width="450" height="450">

- **F1-Confidence Curve**:
- <img src="https://github.com/andreaFaccenda00/DeepVisionAnalytics/assets/171338421/cd6f98dc-c242-4d07-90ee-3c10da2c9f4f" width="450" height="450">

- **Training Metrics**:
- <img src="https://github.com/andreaFaccenda00/DeepVisionAnalytics/assets/171338421/f49bff86-40cf-47de-acb6-dd8d476824e9" width="1200" height="450">

These results illustrate the model's accuracy in detecting pedestrians, its confidence at various thresholds, and the improvements in training metrics over time.

## Architecture Modifications

In the "Dwell Time Analysis for People Flow" project, the YOLOv8 neural network architecture was modified for improved small object detection and computational efficiency:

1. **Parameter Reduction**:
   The network was reduced from 11 million to 2.2 million parameters, enhancing computational efficiency and image processing speed.

2. **Depth Reduction**:
   The network depth was limited to 90 layers, optimizing execution speed while maintaining small object detection capability.

3. **Layer Removal**:
   Layers responsible for medium and large object detection were removed, reducing model complexity and parameters.

4. **Gradient Flow Optimization**:
   Unnecessary connections were reduced to improve gradient flow efficiency, making the model lighter and faster.

5. **Small Object Focus**:
   The modified structure was optimized to detect small objects, the main focus of this use case.

- **Yolov8s Architecture modify**:
- <img src="https://github.com/user-attachments/assets/885730bf-7f9f-498d-b92f-53d9966f591e" width="650" height="650">


### Evaluation Metrics for YOLOv8s (Small Object)

The performance of the modified YOLOv8s network, optimized for small object detection, was evaluated with the following metrics:

- **Box Precision (P)**: 0.826
- **Recall (R)**: 0.641
- **mAP@50**: 0.766
- **mAP@50-95**: 0.47

These modifications resulted in significant inference speed improvements, from 30 fps to 45 fps, with a reduction in parameters from 11.2 million to 2.2 million, making the model optimal for real-time applications. Despite a slight decrease in performance metrics, the speed improvement was a crucial priority.


## 🎬 video processing

### `ultralytics_static_video.py`

Script to run object detection on a video file using the Ultralytics YOLOv8 model.Key parameters include:

  - `--zone_configuration_path`: Path to the zone configuration JSON file.
  - `--source_video_path`: Path to the source video file.
  - `--weights`: Path to the model weights file. Default is `'yolov8s_pedestrian.pt'`.
  - `--device`: Computation device (`'cpu'`, `'mps'` or `'cuda'`). Default is `'cuda'`.
  - `--classes`: List of class IDs to track. If empty, all classes are tracked.
  - `--confidence_threshold`: Confidence level for detections (`0` to `1`). Default is `0.3`.
  - `--iou_threshold`: IOU threshold for non-max suppression. Default is `0.7`.

#### Running the Code
To run this code, ensure you have all the required libraries installed and the correct file paths set for your video, configuration, and model weights. Execute the script as follows:
```bash
python ultralytics_static_video.py --zone_configuration_path data/people.mp4 --source_video_path data/config.json --weights 'yolov8s_pedestrian.pt' --device 'cuda' --classes 0 --confidence_threshold 0.3 --iou_threshold 0.7

```

The script will process the video, detect and track objects, annotate zones of interest, and calculate the time spent in each zone. The output will be saved as an annotated video.

#### video analysis


https://github.com/andreaFaccenda00/DeepVisionAnalytics/assets/171338421/e3c6f94c-3f90-44e5-8dc3-6f77dd6e25a4

### `ultralytics_stream_example.py`
Script to run object detection on a video stream using the Roboflow Inference model.

  - `--zone_configuration_path`: Path to the zone configuration JSON file.
  - `--rtsp_url`: Complete RTSP URL for the video stream.
  - `--weights`: Path to the model weights file. Default is `'yolov8s_pedestrian.pt'`.
  - `--device`: Computation device (`'cpu'`, `'mps'` or `'cuda'`). Default is `'cuda'`.
  - `--confidence_threshold`: Confidence level for detections (`0` to `1`). Default is `0.3`.
  - `--iou_threshold`: IOU threshold for non-max suppression. Default is `0.7`.

```bash
python ultralytics_stream_example.py 
--zone_configuration_path "data/config.json" 
--rtsp_url "rtsp://localhost:8554/live0.stream" 
--weights "yolov8s_pedestrian.pt" 
--device "cuda" 
--classes 0
--confidence_threshold 0.3 
--iou_threshold 0.7
```
## Contributions
Contributions are welcome! Please open an issue or submit a pull request for any improvements or features.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.





