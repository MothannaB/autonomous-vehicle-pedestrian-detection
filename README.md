# Real-Time Pedestrian Detection for Autonomous Driving Using YOLOv3, ROS2, and CARLA

## Overview

In this project, I developed a real-time object detection system for an autonomous vehicle simulation using **YOLOv3**, **ROS2**, and the **CARLA Simulator**. The system continuously processes images from the vehicle's front-facing camera, detects pedestrians, publishes a detection flag to other ROS2 nodes, and saves annotated images for analysis.

The project demonstrates how I integrated computer vision, robotics middleware, and autonomous vehicle simulation into a complete perception pipeline.

---

## Features

* I subscribed to the ego vehicle's front RGB camera in CARLA using ROS2.
* I processed each camera frame using the YOLOv3 object detection model.
* I detected pedestrians in real time with confidence filtering.
* I applied Non-Maximum Suppression (NMS) to eliminate duplicate detections.
* I published a ROS2 detection flag whenever a pedestrian was detected.
* I visualized detections by drawing bounding boxes on each frame.
* I automatically saved annotated images for debugging and performance evaluation.
* I connected directly to the CARLA simulator through its Python API.

---

## Technologies Used

* Python
* ROS2 (rclpy)
* CARLA Simulator
* YOLOv3
* OpenCV DNN
* NumPy
* CvBridge
* ROS2 Image Messages
* ROS2 Publishers and Subscribers

---

## Repository Structure

```text
.
├── yolov3.cfg                 # YOLOv3 network configuration
├── yolov3.weights             # Pre-trained YOLOv3 weights
├── coco.names                 # COCO object class labels
├── pedestrian_detection.py    # ROS2 perception node
├── saved_images/              # Annotated detection images
└── README.md
```

---

## System Architecture

```text
CARLA Simulator
       │
       ▼
Front RGB Camera
       │
       ▼
ROS2 Image Topic
       │
       ▼
CvBridge
       │
       ▼
OpenCV Image
       │
       ▼
YOLOv3 Neural Network
       │
       ▼
Pedestrian Detection
       │
       ▼
Non-Maximum Suppression
       │
       ├───────────────► Publish Detection Flag
       │
       ▼
Draw Bounding Boxes
       │
       ▼
Save Detection Image
```

---

## How It Works

### 1. ROS2 Node Initialization

I created a ROS2 node that subscribes to the RGB camera mounted on the ego vehicle in the CARLA simulator. The node continuously receives camera frames for processing.

---

### 2. Connecting to CARLA

I connected the application to a running CARLA simulation using the CARLA Python API. This enabled the perception node to receive real-time sensor data from the simulated autonomous vehicle.

---

### 3. Loading the YOLOv3 Model

Before processing any images, I loaded:

* the YOLOv3 network configuration (`yolov3.cfg`)
* the pre-trained model weights (`yolov3.weights`)
* the COCO object class labels (`coco.names`)

These files allow the system to recognize the 80 object categories included in the COCO dataset.

---

### 4. Image Processing Pipeline

Whenever a new camera frame arrives, I:

1. Converted the ROS2 image message into an OpenCV image using CvBridge.
2. Resized and normalized the image.
3. Constructed an input blob suitable for YOLOv3.
4. Performed a forward pass through the neural network.
5. Extracted detection results.

---

### 5. Pedestrian Detection

For every detected object, I:

* identified the predicted object class,
* extracted the confidence score,
* filtered out low-confidence detections, and
* retained only detections classified as **person**.

This reduced false positives while focusing on pedestrians relevant to autonomous driving.

---

### 6. Non-Maximum Suppression

Since YOLO frequently predicts multiple overlapping bounding boxes for the same object, I applied **Non-Maximum Suppression (NMS)** to remove redundant detections while preserving the most confident prediction.

---

### 7. Publishing Detection Events

Whenever one or more pedestrians were detected, I published a Boolean message on a ROS2 topic indicating that a pedestrian had been found.

This allows other autonomous driving modules—such as planning or vehicle control—to react appropriately.

---

### 8. Visualization

For every confirmed detection, I:

* drew a bounding box around the pedestrian,
* displayed the confidence score, and
* saved the annotated frame to disk for later inspection.

These saved images can be used for debugging, testing, and evaluating detection performance.

---

## ROS2 Topics

### Subscribed Topics

| Topic                                | Message Type        | Purpose                           |
| ------------------------------------ | ------------------- | --------------------------------- |
| `/carla/ego_vehicle/rgb_front/image` | `sensor_msgs/Image` | Receives camera images from CARLA |

### Published Topics

| Topic                          | Message Type    | Purpose                                       |
| ------------------------------ | --------------- | --------------------------------------------- |
| `/carla/ego_vehicle/stop_flag` | `std_msgs/Bool` | Indicates that a pedestrian has been detected |

---

## Skills Demonstrated

Through this project, I demonstrated experience with:

* Computer Vision
* Deep Learning
* Object Detection
* YOLOv3
* ROS2
* Autonomous Vehicle Perception
* CARLA Simulation
* OpenCV
* Python
* Robotics Software Development
* Real-Time Image Processing
* Sensor Integration
* ROS2 Messaging
* Bounding Box Post-Processing
* Non-Maximum Suppression


