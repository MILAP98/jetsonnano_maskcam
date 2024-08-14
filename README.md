# MaskCam - Real-time Mask Detection System

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Jetson Nano](https://img.shields.io/badge/Platform-Jetson%20Nano-green.svg)](https://developer.nvidia.com/embedded/jetson-nano)

## Introduction

### Aim of This Repository

The primary goal of this repository is to provide a starting point for anyone looking to explore AI and object detection on the NVIDIA Jetson Nano. Whether you're a beginner or an experienced developer, this project aims to help you quickly set up a real-time detection system, making it easier to start learning and experimenting with AI at the edge.

### What You Can Achieve with MaskCam

**MaskCam** is a prototype reference design for a Jetson Nano-based smart camera system that measures face mask usage in real-time. All AI computations are performed at the edge, allowing for efficient and immediate detection without relying on cloud resources. MaskCam detects and tracks people in its field of view, determining whether they are wearing a mask using object detection and tracking algorithms.

Key capabilities include:
- **Real-Time Mask Detection**: Accurately detects and tracks individuals, identifying mask usage in real-time.
- **Edge AI Processing**: Runs entirely on the Jetson Nano, eliminating the need for cloud-based processing and ensuring quick response times.
- **Optional Video Streaming**: Capable of streaming video via RTSP, which can be viewed on VLC or similar players, providing a live view of detections.
- **Versatile Deployment**: Can be run on a Jetson Nano Developer Kit or Jetson Nano module, compatible with any Linux-supported USB webcam.

By using MaskCam, you can leverage the power of edge AI on the Jetson Nano, making it an excellent tool for developing, testing, and deploying real-time detection models in various environments.

## Setup Instructions

### Flashing and Installing JetPack

1. **Download JetPack**:
   - Visit the [NVIDIA JetPack page](https://developer.nvidia.com/embedded/jetpack) and download the latest version compatible with Jetson Nano (JetPack 4.5).

2. **Flash the SD Card**:
   - Use [Etcher](https://www.balena.io/etcher/) to flash the downloaded JetPack image onto a microSD card (minimum 32GB recommended).

3. **Boot the Jetson Nano**:
   - Insert the flashed SD card into the Jetson Nano, connect a monitor, keyboard, and mouse, and power it on.
   - Follow the on-screen instructions to complete the initial setup.

### Setting Up SSH Access

1. **Enable SSH**:
   - During the initial Jetson Nano setup, ensure that SSH is enabled.
   - Find the IP address of your Jetson Nano using `ifconfig`.

2. **Access the Nano via SSH**:
   - From another computer, open a terminal and run:
     ```bash
     ssh username@<your-jetson-ip>
     ```
   - Replace `username` with your Jetson Nano username and `<your-jetson-ip>` with its IP address.

### System Update and Software Installation

1. **Update System Packages**:
   - After logging in via SSH, update the system packages:
     ```bash
     sudo apt-get update
     sudo apt-get upgrade
     ```

2. **Install Python 3 and Pip**:
   - Install Python 3 and the package manager pip:
     ```bash
     sudo apt-get install python3-pip
     ```

3. **Install OpenCV**:
   - Install OpenCV, a critical library for computer vision applications:
     ```bash
     sudo apt-get install libopencv-dev python3-opencv
     ```

### Running MaskCam from a Container on Jetson Nano

The quickest way to get MaskCam running on your Jetson Nano Dev Kit is by using our pre-built Docker container. Here's what you'll need:

1. **Jetson Nano Dev Kit** with JetPack 4.5
2. **External Power Supply**: A DC 5V, 4A power supply connected via the barrel jack.
3. **USB Webcam**: Attach a Logitech USB webcam to the Nano.
4. **RTSP Stream Viewer**: Another computer with an RTSP stream viewer like VLC.

### Steps to Run:

1. **Pull the Container**:
   ```bash
   sudo docker pull maskcam/maskcam-beta
