
# MaskCam - Real-time Mask Detection System
<img width="951" alt="Screenshot 2024-08-14 182340" src="https://github.com/user-attachments/assets/bcc477d2-51c4-40e2-8725-bb6052919f26">

## Introduction

### Aim of This Repository

The primary goal of this repository is to provide a starting point for anyone looking to explore AI and object detection on the NVIDIA Jetson Nano. Whether you're a beginner or an experienced developer, this project aims to help you quickly set up a real-time detection system, making it easier to start learning and experimenting with Jetson Nano.

### What You Can Achieve with MaskCam

MaskCam is a prototype reference design for a Jetson Nano-based smart camera system that measures face mask usage in real-time. All AI computations are performed on jetson nano, allowing for efficient and immediate detection without relying on cloud resources. MaskCam detects and tracks people in its field of view, determining whether they are wearing a mask using object detection and tracking algorithms.

Key capabilities include:

- **Real-Time Mask Detection**: Accurately detects and tracks individuals, identifying mask usage in real-time.
- **Edge AI Processing**: Runs entirely on the Jetson Nano, eliminating the need for cloud-based processing and ensuring quick response times.
- **Video Streaming**: Capable of streaming video via RTSP, which can be viewed on VLC , providing a live view of detections.
- **Versatile Deployment**: Can be run on a Jetson Nano Developer Kit or Jetson Nano module

By using MaskCam, you can leverage the power of edge AI on the Jetson Nano, making it an excellent tool for developing, testing, and deploying real-time detection models in various environments.

## Table of Contents

1. [Hardware Requirements](#hardware-requirements)
2. [Setting Up the Jetson Nano](#setting-up-the-jetson-nano)
3. [Installing PyTorch with GPU Support](#installing-pytorch-with-gpu-support)
4. [Downloading YOLOv5](#downloading-yolov5)
5. [Setting Up the USB CAM](#setting-up-the-USB-cam)
6. [Running YOLOv5 for Mask Detection](#running-yolov5-for-mask-detection)
7. [Training a Custom Model](#training-a-custom-model)
8. [Running the Custom Model](#running-the-custom-model)
9. [References](#references)

## Setting Up the Jetson Nano

### Flashing and Installing JetPack

1. **Download JetPack**:

   Visit the [NVIDIA JetPack page](https://developer.nvidia.com/embedded/jetpack) and download the latest version compatible with Jetson Nano (JetPack 4.5).

2. **Flash the SD Card**:

   Use [Etcher](https://www.balena.io/etcher/) to flash the downloaded JetPack image onto a microSD card (minimum 32GB recommended).

3. **Boot the Jetson Nano**:

   Insert the flashed SD card into the Jetson Nano, connect a monitor, keyboard, and mouse, and power it on.
   Follow the on-screen instructions to complete the initial setup.

### Setting Up SSH Access

1. **Enable SSH**:

   During the initial Jetson Nano setup, ensure that SSH is enabled.
   
   ```python
   sudo systemctl enable ssh
 
   sudo systemctl start ssh.

Find the IP address of your Jetson Nano using `ifconfig`.

3. **Access the Nano via SSH**:

   From another computer, open a terminal and run:
   ```python
   ssh username@<your-jetson-ip>
   ```
   Replace `username` with your Jetson Nano username and `<your-jetson-ip>` with its IP address.

### System Update and Software Installation

1. **Update System Packages**:

   After logging in via SSH, update the system packages:
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```

2. **Install Python 3 and Pip**:

   Install Python 3 and the package manager pip:
   ```bash
   sudo apt-get install python3-pip
   ```
## Installing PyTorch with GPU Support

1. **Install PIP**: Install Python's package manager using the following commands:
   ```bash
   sudo apt install curl
   curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
   sudo python3 get-pip.py
   ```

2. **Install PyTorch**: Download and install PyTorch with CUDA support:
   ```bash
   sudo apt-get install libopenblas-base libopenmpi-dev
   curl -LO https://nvidia.box.com/shared/static/p57jwntv436lfrd78inwl7iml6p13fzh.whl
   mv p57jwntv436lfrd78inwl7iml6p13fzh.whl torch-1.8.0-cp36-cp36m-linux_aarch64.whl
   sudo pip3 install torch-1.8.0-cp36-cp36m-linux_aarch64.whl
   ```

3. **Verify Installation**: Confirm that PyTorch has been installed successfully:
   ```bash
   sudo python3 -c "import torch; print(torch.cuda.is_available())"
   ```

4. **Install torchvision**: Build and install torchvision:
   ```bash
   sudo apt install libjpeg-dev zlib1g-dev
   git clone --branch v0.9.1 https://github.com/pytorch/vision torchvision
   cd torchvision/
   sudo python3 setup.py install
   cd ..
   ```

## Downloading YOLOv5

1. **Clone YOLOv5 Repository**:
   ```bash
   git clone https://github.com/ultralytics/yolov5.git
   cd yolov5
   ```

2. **Install Dependencies**:
   ```bash
   export OPENBLAS_CORETYPE=ARMV8
   sudo apt install libfreetype6-dev python3-dev
   sudo pip3 install numpy==1.19.4
   sudo pip3 install --ignore-installed PyYAML>=5.3.1
   sudo pip3 install -r requirements.txt
   ```

3. **Test YOLOv5**: Run a test to verify the installation:
   ```bash
   sudo python3 detect.py
   ```

2. **Find Your Nano's IP Address**:

   Use `ifconfig` to find your local Jetson Nano IP address.

3. **Run MaskCam**:

   ```bash
   sudo docker run --runtime nvidia --privileged --rm -it --env MASKCAM_DEVICE_ADDRESS=<your-jetson-ip> -p 1883:1883 -p 8080:8080 -p 8554:8554 maskcam/maskcam-beta
   ```

### Viewing the Live Video Stream with VLC

1. **Start MaskCam**: Run the MaskCam container and check the logs for the RTSP URL:

   ```bash
   Streaming at rtsp://<your-jetson-ip>:8554/maskcam
   ```

2. **Open VLC**:

   - On your other computer, open VLC Media Player.
   - Go to `Media > Open Network Stream`.

3. **Enter the RTSP URL**:

   - Paste the RTSP stream URL into the “Network URL” field.
   - Click `Play`.

4. **Watch the Stream**:
   
 [![Watch the video](https://img.youtube.com/vi/xdeqE5vwJhM/hqdefault.jpg)](https://www.youtube.com/watch?v=xdeqE5vwJhM&autoplay=1)

   VLC will start streaming the live video from your Jetson Nano, showing real-time mask detection with green boxes for masked faces and red boxes for unmasked faces.

This allows you to  view the output of MaskCam on any computer within the same network.

## Acknowledgements

- **YOLOv5**: Special thanks to the creators of YOLOv5 for providing a powerful object detection framework.
- **NVIDIA Jetson**: Thanks to NVIDIA for the Jetson platform, which makes edge computing possible.
- **Open Source Libraries**: This project relies on numerous open-source libraries and tools.
- This project is heavily inspired by and based on the work from the [MaskCam repository by bdtinc](https://github.com/bdtinc/maskcam/tree/main/maskcam).

 

### References

For more information, you can visit the original [MaskCam repository](https://github.com/bdtinc/maskcam/tree/main/maskcam) by bdtinc. This repository contains detailed documentation, examples, and the original implementation of the MaskCam project.
We extend our gratitude to the creators of the original MaskCam project for their hard work and contributions to the community.

