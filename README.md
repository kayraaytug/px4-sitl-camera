# PX4 SITL Camera

Provides a binary that captures camera frames from the Gazebo simulation environment and redirects them to GStreamer. This allows the frames to be used with OpenCV or other tools that support GStreamer pipelines.

Thanks to [Jonas Vautherin's px4-gazebo-headless](https://github.com/JonasVautherin/px4-gazebo-headless) for resources.

## Features
- Captures camera frames from Gazebo.
- Redirects the frames to GStreamer for further processing.

## Build Instructions
1. Navigate to the project directory.
2. Create a build directory and run CMake:
   ```bash
   sudo apt install libgstrtspserver-1.0-0
   mkdir build && cd build
   cmake ..
   make
   ```
3. The binary will be available in the `build` directory.

## Usage
### Start the PX4 SITL
```bash
cd PX4-Autopilot 
make px4_sitl gz_x500_mono_cam
```

### Run the Camera Proxy
Open another terminal and execute:
```bash
cd px4-sitl-camera
./build/sitl_rtsp_proxy
```

### View the Stream
To see the stream, use the following GStreamer pipeline:
```bash
gst-launch-1.0 rtspsrc location=rtsp://127.0.0.1:8554/live ! rtph264depay ! avdec_h264 ! videoconvert ! autovideosink
```

### Use with OpenCV
To integrate with OpenCV, use this pipeline:
```cpp
constexpr const char* gst_pipeline = "rtspsrc location=rtsp://127.0.0.1:8554/live ! rtph264depay ! avdec_h264 ! videoconvert ! appsink";

cv::VideoCapture cap(gst_pipeline, cv::CAP_GSTREAMER);
```

## Requirements
- GStreamer
- libgstrtspserver-1.0-0

## License
This project is licensed under the MIT License.

