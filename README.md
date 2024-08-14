
# MaskCam - Real-time Mask Detection System

## Introduction

### Aim of This Repository

The primary goal of this repository is to provide a starting point for anyone looking to explore AI and object detection on the NVIDIA Jetson Nano. Whether you're a beginner or an experienced developer, this project aims to help you quickly set up a real-time detection system, making it easier to start learning and experimenting with AI at the edge.

### What You Can Achieve with MaskCam

MaskCam is a prototype reference design for a Jetson Nano-based smart camera system that measures face mask usage in real-time. All AI computations are performed at the edge, allowing for efficient and immediate detection without relying on cloud resources. MaskCam detects and tracks people in its field of view, determining whether they are wearing a mask using object detection and tracking algorithms.

Key capabilities include:

- **Real-Time Mask Detection**: Accurately detects and tracks individuals, identifying mask usage in real-time.
- **Edge AI Processing**: Runs entirely on the Jetson Nano, eliminating the need for cloud-based processing and ensuring quick response times.
- **Optional Video Streaming**: Capable of streaming video via RTSP, which can be viewed on VLC or similar players, providing a live view of detections.
- **Versatile Deployment**: Can be run on a Jetson Nano Developer Kit or Jetson Nano module, compatible with any Linux-supported USB webcam.

By using MaskCam, you can leverage the power of edge AI on the Jetson Nano, making it an excellent tool for developing, testing, and deploying real-time detection models in various environments.

## Setup Instructions

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
   Find the IP address of your Jetson Nano using `ifconfig`.

2. **Access the Nano via SSH**:

   From another computer, open a terminal and run:
   ```bash
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

3. **Install OpenCV**:

   Install OpenCV, a critical library for computer vision applications:
   ```bash
   sudo apt-get install libopencv-dev python3-opencv
   ```

### Running MaskCam from a Container on Jetson Nano

The quickest way to get MaskCam running on your Jetson Nano Dev Kit is by using our pre-built Docker container. Here's what you'll need:

- Jetson Nano Dev Kit with JetPack 4.5
- External Power Supply: A DC 5V, 4A power supply connected via the barrel jack.
- USB Webcam: Attach a Logitech USB webcam to the Nano.
- RTSP Stream Viewer: Another computer with an RTSP stream viewer like VLC.

### Steps to Run:

1. **Pull the Container**:

   ```bash
   sudo docker pull maskcam/maskcam-beta
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

   VLC will start streaming the live video from your Jetson Nano, showing real-time mask detection with green boxes for masked faces and red boxes for unmasked faces.

This allows you to remotely view the output of MaskCam on any computer within the same network.

## Troubleshooting

If you encounter issues, refer to the [Troubleshooting Common Errors](https://github.com/bdtinc/maskcam/blob/main/README.md#troubleshooting-common-errors) section for tips on resolving common problems such as camera recognition failures, streaming errors, and more.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss your ideas or improvements.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- **YOLOv5**: Special thanks to the creators of YOLOv5 for providing a powerful object detection framework.
- **NVIDIA Jetson**: Thanks to NVIDIA for the Jetson platform, which makes edge computing possible.
- **Open Source Libraries**: This project relies on numerous open-source libraries and tools.
