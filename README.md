A real-time intelligent surveillance system that processes multiple video streams using **YOLOv7**, **RabbitMQ**, **Redis**, **OpenCV**, and **Python**. The project performs object detection and person segmentation while validating detections inside predefined Regions of Interest (ROI) to improve monitoring accuracy and reduce unnecessary event storage.

---

## 📌 Project Overview

This project is designed to build a scalable video analytics platform capable of handling multiple surveillance cameras simultaneously. Instead of processing every detected object, the system checks whether the detection falls within a user-defined Region of Interest (ROI). Only valid detections are processed and stored.

The application follows a distributed producer-consumer architecture where RabbitMQ manages communication between different processing modules and Redis stores configurations and detection events for faster access.

---

## ✨ Features

* Real-time object detection using a custom-trained YOLOv7 model
* Person segmentation with YOLOv7 Segmentation
* ROI (Region of Interest) based validation
* Multi-camera surveillance support
* Distributed communication using RabbitMQ
* Redis-based configuration and event storage
* Parallel processing using multiprocessing and multithreading
* Detection of vehicles, people, fire, smoke, helmets, and safety jackets

---

## 🎯 Project Goal

The main objective of this project is to develop an efficient surveillance system that focuses only on important regions within a video frame.

This helps to:

* Reduce false detections
* Minimize unnecessary processing
* Save storage space
* Improve overall detection accuracy
* Support real-time monitoring across multiple cameras

---

## 🧠 Deep Learning Model

### Dataset Preparation

A custom dataset was collected containing images of:

* Person
* Car
* Bike
* Helmet
* Safety Jacket
* Fire
* Smoke

### Image Annotation

The dataset was annotated using **LabelImg**, where bounding boxes were created for every object.

### Model Training

The annotated dataset was trained using YOLOv7 to generate a custom detection model.

**Output Model**

```
best.pt
```

This trained model is used for real-time inference.

---

## 📂 Detection Classes

| ID | Object |
| -- | ------ |
| 0  | Person |
| 1  | Car    |
| 2  | Bike   |
| 3  | Helmet |
| 4  | Jacket |
| 5  | Fire   |
| 6  | Smoke  |

---

## ⚙️ System Workflow

### Step 1 — Camera Configuration

Camera details such as camera ID, video path, IP address, and supported labels are stored inside a JSON configuration file.

Example:

```json
{
  "camera_1": {
    "ip_address": "192.168.1.100",
    "video_path": "video1.mp4",
    "labels": ["car", "bike"]
  }
}
```

---

### Step 2 — Configuration Management

When the application starts, it first checks Redis for existing camera configurations.

* If configuration exists, it is loaded directly.
* Otherwise, the JSON file is read and stored in Redis for future use.

---

### Step 3 — Frame Acquisition

Each camera stream runs independently using multiprocessing and multithreading.

Frames are continuously captured from every camera.

---

### Step 4 — Frame Publishing

Captured frames are encoded using Base64 and published to RabbitMQ.

This enables different services to process frames asynchronously without directly depending on one another.

---

### Step 5 — Queue Distribution

Frames are separated into different queues based on processing requirements.

* Person Queue
* Object Detection Queue

This separation allows segmentation and detection to run independently.

---

### Step 6 — Object Detection

YOLOv7 detects multiple object categories including:

* Person
* Car
* Bike
* Helmet
* Jacket
* Fire
* Smoke

Each detection contains:

* Class Label
* Bounding Box
* Confidence Score

---

### Step 7 — Person Segmentation

Frames containing people are processed using YOLOv7 Segmentation.

The model generates segmentation masks for more accurate human detection.

---

### Step 8 — Detection Routing

After inference, detection results are routed through RabbitMQ into specialized queues such as:

* Vehicle Detection
* Fire & Smoke Detection
* PPE Monitoring

This modular architecture makes the system easier to scale.

---

### Step 9 — ROI Validation

Every detected object is checked against predefined ROI coordinates.

Objects detected:

* Inside ROI → Stored
* Outside ROI → Ignored

This reduces false alarms and focuses only on relevant activities.

---

### Step 10 — Event Storage

Validated events are stored in Redis along with:

* Camera ID
* Timestamp
* Detection Class
* Bounding Box
* Confidence Score
* ROI Status

---

## 🏗️ Project Architecture

```
Camera Streams
       │
       ▼
Configuration (JSON)
       │
       ▼
Redis
       │
       ▼
Frame Producer
       │
       ▼
RabbitMQ
       │
 ┌───────────────┐
 │               │
 ▼               ▼
Person Queue   Detection Queue
 │               │
 ▼               ▼
YOLOv7 Seg      YOLOv7 Detection
 │               │
 └──────┬────────┘
        ▼
ROI Validation
        │
        ▼
Redis Event Storage
```

---

## 📊 Applications

* Smart Surveillance
* Industrial Worker Safety
* PPE Compliance Monitoring
* Vehicle Monitoring
* Fire and Smoke Detection
* Restricted Area Monitoring
* Smart City Surveillance

---

## 🛠️ Technology Stack

| Technology          | Purpose                       |
| ------------------- | ----------------------------- |
| Python              | Application Development       |
| OpenCV              | Video Processing              |
| YOLOv7              | Object Detection              |
| YOLOv7 Segmentation | Person Segmentation           |
| RabbitMQ            | Message Queue                 |
| Redis               | Configuration & Event Storage |
| LabelImg            | Dataset Annotation            |
| Multiprocessing     | Parallel Processing           |
| Multithreading      | Camera Stream Handling        |

---

## 🚀 Future Improvements

* Object Tracking using DeepSORT
* Vehicle Counting
* Automatic Alert Generation
* Email & SMS Notifications
* Web Dashboard
* Docker Containerization
* Kubernetes Deployment
* Cloud Deployment (AWS, Azure, GCP)

---

## 📈 Project Highlights

* Developed a distributed video analytics pipeline
* Trained a custom YOLOv7 model for multiple object classes
* Implemented ROI-based event filtering
* Integrated RabbitMQ for asynchronous communication
* Used Redis for efficient storage and configuration management
* Supported simultaneous processing of multiple camera feeds

---

## 📚 Conclusion

This project demonstrates a scalable and intelligent video analytics system that combines computer vision with distributed messaging. By integrating YOLOv7, RabbitMQ, Redis, and ROI-based filtering, the system efficiently monitors multiple camera streams while processing only relevant events.

The architecture is modular, scalable, and suitable for real-world surveillance applications where speed, accuracy, and resource optimization are important.

---

## 👩‍💻 Author

**Yamani Swathi**

AI & Machine Learning Enthusiast

📧 Email: *swathiyamani2004@gmail.com*

🔗 LinkedIn: *https://www.linkedin.com/in/swathi-yamani-8a90a8232/*
