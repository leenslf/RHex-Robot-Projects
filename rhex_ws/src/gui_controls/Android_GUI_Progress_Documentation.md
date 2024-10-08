# RHex Simulation Android GUI Progress Documentation

## Initial Task Selection

Initially, we selected this task because we believed it could be highly beneficial and relevant to both robotics and software front-end engineering. Our journey began by exploring the RhexAPI.h and understanding the underlying workings of the GUI.


## Phase 1: Android Studio Project
In the first week, we used an existing statically linked library containing precompiled binaries of functions that run the RHex simulation controller. This exploration involved examining basic_walk.cc, a script utilizing RhexAPI along with frameworks and libraries such as GStreamer, GLib, OpenSSL, and Argon2.

Our initial approach involved creating an Android Studio project with all dependencies, including RhexAPI and other required libraries and frameworks. 

### Challenges Faced

1. *GStreamer Libraries:* We encountered difficulties installing architecture-compatible GStreamer libraries. These were finally installed from the GStreamer website and linked all dependencies in the makefile.
2. *Library Compatibility:* The RhexAPI statically linked library version we had was not compatible with ARM64v8. After updating it, we faced issues with C++ version incompatibility and further GStreamer library incompatibilities with our development machine.

Due to these compilation challenges, we decided to try a more efficient approach.

## Phase 2: Server-Client Communication Approach

We shifted to a server-client communication model to continue the project. This approach involves setting up communication between an Android device and a host PC containing the container where the RHex simulation runs.

### Implementation Steps

1. *Flask Server Setup:* 
    - We set up a Flask server inside the container, making changes to the Dockerfile to include necessary dependencies and precompiled binaries.
    - We opened a port (5001) for communication between the container and the host, ensuring the host machine allows communication through this port.

2. *Communication Establishment:*
    - The Android device communicates with the Flask server on the host PC to run precompiled files with compatible dependencies.
    - The user inputs the IP address on the Android app to establish this communication.

3. *Yolo Human Detection Integration:*
    - We integrated the YOLO human detection model, initially intended to process image topics outside the container using ROSBridge.
    - To improve efficiency and avoid redundancy, we decided to export YOLO-processed images directly to the Android project, maintaining simplicity and utilizing existing YOLO annotations.
    - We ensured that the image annotations were visible by using *thread locks* to manage image updates, avoiding issues with annotation visibility.

### Background Workflow

- *Flask Server Control Client:*
    - The Flask server listens for commands from the Android client.
    - Upon receiving a command, it processes it and interacts with the control client to execute necessary actions on the RHex robot.

- *Client Server Interaction:*
    - The control client sends commands to the control server running on the host PC.
    - The control server, in turn, communicates with the RHex robot to perform actions such as connecting, standing, sitting, or walking.

- *Image Processing:*
    - The image processing node subscribes to the camera image topic, processes images using YOLO, and updates the global image variable.
    - This image is then encoded and sent to the Android client, where it is displayed with YOLO annotations.

## Phase 3: PyrhexAPI Integration and Simplification

In the final week of our internship, we tested and documented PyrhexAPI. Utilizing PyrhexAPI with PyBind in Python allowed us to **eliminate the need for client_controller and client_server C++ files**.

### PyBind Usage
PyBind facilitates seamless integration between C++ and Python, enabling direct control of the RHex robot via Python scripts without intermediate C++ files.

### Benefits and Implementation
- *Simplified Setup:* Only one Python script is needed to initiate the application, significantly reducing setup complexity.
- *Efficiency:* PyrhexAPI integration streamlined our project, eliminating compatibility issues and compilation challenges of C++ files.
- *Streamlined Workflow:* This approach accelerated iterations and simplified debugging.
- *Cross-Platform Compatibility:* By using PyBind, we avoid the struggle of compiling C++ files for different architectures.

Implementing PyrhexAPI with PyBind resulted in a more efficient and maintainable solution for RHex robot control and image processing.


## Conclusion

Through this iterative process, we overcame multiple technical challenges and achieved a robust communication framework between the Android device and the RHex simulation. 

## Contributors
- Hamzeh Awad
- Leen Said

## Downloads

- [Download APK](https://drive.google.com/file/d/1UDs2m8riKrxz9fJDtkoOsUQVV0DAIuva/view?usp=drive_link)
- [Download Android Studio Project (ZIP)](https://drive.google.com/file/d/1qI6UYGzsGY2vcx9QC1JV_rZVzE571K9q/view?usp=drive_link)