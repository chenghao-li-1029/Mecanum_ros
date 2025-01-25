# Table of Contents
- [1. Introduction](#1-introduction)
  - [1.1 Overview](#11-overview)
  - [1.2 Key Features](#12-key-features)
  - [1.3 Research Applications](#13-research-applications)
  - [1.4 Technical Contributions](#14-technical-contributions)
- [2. Hardware Architecture](#2-hardware-architecture)
  - [2.1 Sensor Suite](#21-sensor-suite)
  - [2.2 Computing Platform](#22-computing-platform)
  - [2.3 Actuation System](#23-actuation-system)
- [3. Software Architecture](#3-software-architecture)
  - [3.1 System Overview](#31-system-overview)
  - [3.2 Algorithm Integration](#32-algorithm-integration)
- [4. Tutorial](#4-tutorial)
  - [4.1 Preparing & Setup](#41-preparingsetup)
  - [4.2 Compile & Build](#42-compile--build)
  - [4.3 ROS Remote Connection](#43-ros-remote-connection)
  - [4.4 2D LIDAR SLAM](#44-2d-lidar-slam)
  - [4.5 Rtabmap 3D VSLAM](#45-rtabmap-3d-vslam)
  - [4.6 Audio](#46-audio)
  - [4.7 Visual Tracking](#47-visual_tracking)
  - [4.8 Simulation (Gazebo)](#48-simulationgazebo)
  - [4.9 SC_GUI](#49-sc_gui)
  - [4.10 Time Synchronization](#410-time-synchronization)
  - [4.11 Auto Driving Demo](#411-auto-driving-demo)
- [5. ROS Packages](#5-ros-packakges)
  - [5.1 sc_hw](#51-sc_hw)
  - [5.2 rplidar](#52-rplidar)
  - [5.3 realsense2_camera](#53-realsense2_camera)
  - [5.4 ocean_vision](#54-ocean_vision)
  - [5.5 sc_2dnav](#55-sc_2dnav)
- [6. Citation](#6-citation)
- [7. Acknowledgments](#7-acknowledgments)

# 1 Introduction

## 1.1 Overview

This document provides comprehensive documentation for an advanced ROS-based robotics software stack that integrates state-of-the-art perception, localization, and navigation algorithms. [Chinese Version](README_CHN.md)

While specifically designed and tested for the **Oceanbotech SmartCar V1.0** - a mecanum wheel omnidirectional platform - this software framework is built on standard ROS interfaces and can be adapted to other differential-drive, omnidirectional, or holonomic mobile robots. The modular architecture enables seamless integration with various hardware configurations, making it suitable for both educational purposes and advanced research in mobile robotics, SLAM (Simultaneous Localization and Mapping), computer vision, and 3D reconstruction.

### Research Enablement

This platform has enabled real-world deployment experiments for the following research works:

- **[ESS-SLAM](https://github.com/chenghao-li-1029/ESS-SLAM)**: "Quantized Self-supervised Local Feature for Real-time Robot Indirect VSLAM" - A self-supervised feature-based VSLAM system utilizing lightweight neural networks for real-time feature detection and description, integrated with the Mecanum platform for practical robotic navigation experiments.

- **[S2LD](https://github.com/chenghao-li-1029/S2LD)**: "Sparse-to-Local-Dense Matching for Geometry-Guided Correspondence Estimation" - Advanced geometric correspondence estimation methods validated through the platform's multi-sensor suite, demonstrating robust feature matching in real-world scenarios.

- **[Online NeRF](https://chenghao-li-1029.github.io/onlinenerf/)**: "Representing Boundary-ambiguous Scene Online with Scale-encoded Cascaded Grids and Radiance Field Deblurring" - A novel online scene representation method that simultaneously learns implicit scene representations and estimates camera poses from RGB-D streams, featuring cascaded grids with scale encoding and radiance field deblurring for photo-realistic 3D reconstruction without known camera poses.

- **[Autonomous Exploration](https://www.mdpi.com/1424-8220/20/2/490)**: This platform provided experimental infrastructure and software support for autonomous exploration research, demonstrating the system's capability in real-world deployment scenarios.

The platform's comprehensive sensor suite (RGB-D camera, LiDAR, IMU) and reliable odometry system provide the necessary infrastructure for deploying and validating these cutting-edge SLAM and computer vision algorithms in practical robotic applications.

## 1.2 Key Features

**Hardware Capabilities:**
- **Omnidirectional Mobility**: Mecanum wheel configuration enabling holonomic motion (translation + rotation simultaneously)
- **Multi-Modal Sensing**: Intel RealSense D435i RGB-D camera, RPLiDAR A2 360° laser scanner, 9-axis IMU
- **Onboard Computing**: Industrial-grade NUC with sufficient computational power for real-time SLAM and perception
- **Encoder Feedback**: High-resolution motor encoders for precise odometry estimation

**Software Stack:**
- **ROS Kinetic Integration**: Full ROS ecosystem support with standardized interfaces
- **Multiple SLAM Paradigms**: 2D (gmapping), 2.5D (hybrid), and 3D (rtabmap) SLAM capabilities
- **Visual Servoing**: Real-time object tracking and following using CMT algorithm
- **Navigation Stack**: Complete autonomous navigation with dynamic obstacle avoidance
- **Simulation Support**: Gazebo integration for algorithm validation before deployment

## 1.3 Research Applications

This platform serves as a foundational infrastructure for research in multiple domains:

### **SLAM and Localization**
- **2D Laser SLAM**: Leveraging gmapping for occupancy grid mapping in structured environments
- **Visual SLAM**: RGB-D SLAM using rtabmap with loop closure detection and graph optimization
- **Multi-Sensor SLAM**: Fusion of LiDAR, visual, and inertial measurements for robust localization
- **Dense Mapping**: Generating dense 3D point clouds and meshes for environment reconstruction

### **Computer Vision and Perception**
- **Visual Odometry**: Stereo and RGB-D based ego-motion estimation
- **Object Detection and Tracking**: Real-time visual tracking with depth information
- **Semantic Segmentation**: Integration potential for scene understanding
- **Place Recognition**: Visual loop closure for long-term autonomy

### **3D Reconstruction and Scene Understanding**
- **Dense Surface Reconstruction**: Real-time volumetric integration (TSDF-based)
- **Point Cloud Processing**: Registration, filtering, and feature extraction
- **Occupancy Mapping**: Multi-resolution voxel grids for navigation and planning
- **Neural Implicit Representations**: Platform for testing NeRF, Neural SDF, and Gaussian Splatting in real-world scenarios

### **Autonomous Navigation**
- **Path Planning**: Global (A*, Dijkstra) and local (DWA, TEB) planning algorithms
- **Localization**: Monte Carlo Localization (AMCL) with particle filtering
- **Dynamic Obstacle Avoidance**: Real-time replanning in populated environments
- **Human-Robot Interaction**: Visual and audio-based interaction capabilities

## 1.4 Technical Features

### **Multi-Algorithm Support**
- Integrates multiple SLAM backends: 2D laser SLAM (Cartographer, Gmapping) and 3D visual SLAM (ORB-SLAM3)
- Provides synchronized multi-sensor data (LiDAR, RGB-D, IMU) for algorithm comparison and fusion

### **Research Applications**
- RGB-D streams with pose data support 3D reconstruction research (NeRF, Gaussian Splatting, neural implicit surfaces)
- Sensor recording for dataset collection and learning-based methods
- Omnidirectional mobility for active perception studies

### **Development Benefits**
- Modular ROS architecture with well-defined interfaces
- Gazebo simulation support for reproducible testing
- Open-source hardware and software design

## Prerequisites

Before starting to use Oceanbotech SmartCar V1.0, please ensure:
- Ubuntu 16.04 installed (Ubuntu 18.04 with ROS Melodic also supported)
- Basic familiarity with [ROS concepts and tutorials](http://wiki.ros.org/ROS/Tutorials)
- C++ and Python programming experience recommended
- Understanding of robotics fundamentals (coordinate frames, transformations, etc.)

**Quick Start Example:**
```bash
roslaunch sc_hw sc_hw.launch
roslaunch sc_hw mecanum_keyboard.launch
```

## Environment

**Tested Configurations:**
- Ubuntu 16.04 LTS + ROS Kinetic Kame
- Ubuntu 18.04 LTS + ROS Melodic Morenia (community supported)

**Recommended Hardware:**
- Intel NUC or equivalent (i5/i7, 8GB+ RAM)
- NVIDIA GPU (optional, for deep learning-based perception)

---

# 2 Hardware Architecture

## 2.1 Sensor Suite

### **Intel RealSense D435i RGB-D Camera**
- **Specifications**: 
  - Depth Range: 0.3m - 3m (optimal), up to 10m
  - Depth Resolution: 1280×720 @ 30fps
  - RGB Resolution: 1920×1080 @ 30fps
  - IMU: 6-DOF (accelerometer + gyroscope)
  - Infrared Stereo Baseline: 50mm
- **Applications**: Visual SLAM, dense reconstruction, object detection, visual servoing
- **Interface**: USB 3.0
- **Data Outputs**: 
  - Aligned/unaligned RGB-D streams
  - Point clouds (PointCloud2)
  - IMU data for visual-inertial odometry

### **RPLiDAR A2 360° Laser Scanner**
- **Specifications**:
  - Range: 0.15m - 12m
  - Angular Resolution: 0.9°
  - Sample Rate: 4000 Hz
  - Scan Rate: 5-10 Hz (configurable)
  - Accuracy: ±1.5% of distance
- **Applications**: 2D SLAM, obstacle detection, localization
- **Interface**: USB to Serial (UART)
- **Advantages**: Robust to lighting conditions, wide field of view

### **9-Axis IMU (MPU9250 or equivalent)**
- **Specifications**:
  - 3-axis accelerometer, gyroscope, magnetometer
  - Output Rate: 100+ Hz
  - Integration: Extended Kalman Filter for attitude estimation
- **Applications**: Orientation estimation, sensor fusion, dynamic modeling
- **Benefits**: Complements visual and LiDAR odometry during fast motions

### **Motor Encoders**
- **Type**: Quadrature encoders on each wheel
- **Resolution**: High-precision pulse counting
- **Applications**: Wheel odometry, velocity feedback, motion control
- **Calibration**: Odometry correction factors for systematic error compensation

## 2.2 Computing Platform

### **Intel NUC (Network Unit Computer)**
- **Typical Configuration**:
  - CPU: Intel Core i5/i7 (4+ cores)
  - RAM: 8GB+ DDR4
  - Storage: 256GB+ SSD
  - OS: Ubuntu 16.04/18.04
- **Computational Budget**:
  - Real-time sensor processing (30+ Hz)
  - SLAM backend optimization
  - Local planning and control
- **Connectivity**: WiFi, Ethernet, multiple USB 3.0 ports

### **Remote PC (Optional for Distributed Computing)**
- **Use Cases**:
  - Visualization (RViz, GUI)
  - Computationally intensive tasks (dense reconstruction, neural network inference)
  - Data logging and post-processing
- **Communication**: ROS Master-Slave architecture over network

## 2.3 Actuation System

### **Mecanum Wheels**
- **Configuration**: 4-wheel holonomic drive
- **Kinematics**: Omnidirectional motion (3 DOF: x, y, θ)
- **Advantages**:
  - No need for rotation before translation
  - Precise positioning in constrained spaces
  - Smooth trajectory execution
- **Control**: Independent motor control with velocity feedback

### **Motor Controllers**
- **Type**: Embedded STM32-based controller
- **Communication Protocol**: Custom serial protocol over USB
- **Control Loop**: PID velocity control at 100+ Hz
- **Safety Features**: Emergency stop, battery monitoring, watchdog timer

---

# 3 Software Architecture

## 3.1 System Overview

The software architecture follows a **layered design** paradigm, typical of modern robotic systems:

### **Overall System Data Flow:**

```
┌─────────────────────────────────────────────────────────────────────┐
│                         User Interface Layer                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │
│  │   RViz   │  │  SC_GUI  │  │ Keyboard │  │  Web Dashboard   │  │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────────┬─────────┘  │
└───────┼─────────────┼─────────────┼──────────────────┼─────────────┘
        │             │             │                  │
        ↓             ↓             ↓                  ↓
        └─────────────┴─────────────┴──────────────────┘
                              │
        ┌─────────────────────┴─────────────────────┐
        │            ROS Topics & Services          │
        │  /cmd_vel, /move_base_simple/goal, etc.   │
        └─────────────────────┬─────────────────────┘
                              │
        ┌─────────────────────┴─────────────────────┐
        │                                            │
        ↓                                            ↓
┌──────────────┐                            ┌──────────────┐
│  Navigation  │                            │  Perception  │
│              │                            │              │
│ • move_base  │←───────────/map────────────┤ • gmapping  │
│ • AMCL       │                            │ • rtabmap    │
│ • DWA        │←──────/scan, /cloud────────┤ • tracking  │
└──────┬───────┘                            └──────┬───────┘
       │                                           │
       │ /cmd_vel                                  │ /scan, /image
       │                                           │
       ↓                                           ↓
┌──────────────────────────────────────────────────────┐
│           Robot Hardware Interface (sc_hw)           │
│                                                      │
│  ┌──────────────┐        ┌──────────────┐          │
│  │ Velocity     │───────→│   Serial     │          │
│  │ Controller   │        │   Protocol   │          │
│  └──────────────┘        └──────┬───────┘          │
│                                  │                   │
│  ┌──────────────┐                │ USB              │
│  │ Odometry     │←───────────────┘                  │
│  │ Publisher    │                                    │
│  └──────────────┘                                    │
└──────────────────────────┬───────────────────────────┘
                           │
                           ↓
        ┌──────────────────────────────────┐
        │      Embedded Controller (MCU)    │
        │                                   │
        │  • PID Control (100Hz)            │
        │  • Motor PWM                      │
        │  • Encoder Reading                │
        │  • IMU Processing                 │
        └──────────────────────────────────┘
                           │
                           ↓
        ┌──────────────────────────────────┐
        │      Physical Hardware            │
        │  • Mecanum Wheels                 │
        │  • DC Motors                      │
        │  • Encoders                       │
        └──────────────────────────────────┘

   Sensors (Parallel Data Streams):
   ┌──────────┐  ┌──────────┐  ┌──────────┐
   │ RPLiDAR  │  │ RealSense│  │   IMU    │
   │  /scan   │  │ /camera  │  │ /imu_data│
   └────┬─────┘  └────┬─────┘  └────┬─────┘
        └─────────────┴──────────────┘
                      │
                      ↓ (into perception/SLAM)
```

```
┌─────────────────────────────────────────────────────────────┐
│              Application Layer (User Interface)              │
│         (RViz, SC_GUI, Web Interface, Voice Control)         │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                   Planning & Decision Layer                  │
│    (Navigation Stack, Path Planning, Behavior Trees)        │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                 Perception & Mapping Layer                   │
│  (SLAM: gmapping/rtabmap, Object Detection, Localization)   │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    Sensor Processing Layer                   │
│    (Camera Driver, LiDAR Driver, IMU Fusion, Odometry)      │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                    Hardware Abstraction Layer                │
│         (ROS Controllers, Serial Communication)              │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                         Hardware Layer                       │
│      (Motors, Sensors, Embedded Controllers, Power)         │
└─────────────────────────────────────────────────────────────┘
```

## 3.2 Algorithm Integration

This platform's strength lies in its **seamless integration of multiple state-of-the-art algorithms**:

### **SLAM Algorithms**

#### **1. GMapping (2D Laser SLAM)**
- **Algorithm Type**: Rao-Blackwellized Particle Filter
- **Input**: Laser scans (/scan) + Odometry (/odom)
- **Output**: 2D occupancy grid map
- **Advantages**: 
  - Fast and efficient for structured indoor environments
  - Mature and well-tested
  - Low computational requirements
- **Limitations**: No vertical structure information
- **Research Applications**: Baseline for 2D navigation research

#### **2. RTAB-Map (Real-Time Appearance-Based Mapping)**
- **Algorithm Type**: Graph-based RGB-D SLAM with loop closure
- **Input**: RGB-D images + Odometry + (optional) LiDAR
- **Output**: 
  - 3D point cloud map
  - 2D occupancy grid (projected from 3D)
  - Pose graph with loop closures
- **Key Features**:
  - **Memory Management**: Automatic map size limitation for real-time performance
  - **Loop Closure Detection**: Bag-of-words based visual place recognition
  - **Graph Optimization**: g2o backend for pose graph optimization
  - **Multi-Session Mapping**: Map merging and localization in pre-built maps
- **Technical Details**:
  - Feature Extraction: GFTT, ORB, SURF, or SIFT
  - Descriptor Matching: With geometric consistency check (RANSAC)
  - Optimization: Iterative optimization with robust kernels
- **Research Potential**: Platform for testing novel loop closure methods, map representations

#### **3. Hybrid LiDAR-Visual SLAM**
- **Fusion Strategy**: rtabmap can fuse LiDAR scans for enhanced robustness
- **Benefits**: 
  - Visual features for place recognition
  - LiDAR for geometric consistency
  - Complementary failure modes (vision fails in texture-poor scenes, LiDAR unaffected)

### **Visual Tracking and Servoing**

#### **CMT (Clustering-based Motion Tracking)**
- **Algorithm Type**: Keypoint-based object tracking
- **Features**: 
  - Long-term tracking with online learning
  - Robust to partial occlusion
  - Scale and rotation invariant
- **Control Loop**: 
  - PID controller for robot velocities based on object position
  - Depth-based distance control
- **Applications**: Human following, object manipulation, active vision

### **Navigation Algorithms**

#### **AMCL (Adaptive Monte Carlo Localization)**
- **Algorithm Type**: Particle filter localization
- **Input**: Laser scans + Odometry + Map
- **Output**: Robot pose estimate with uncertainty
- **Adaptive Sampling**: Dynamic particle count based on localization confidence

#### **Move Base (Navigation Stack)**
- **Global Planner**: 
  - Dijkstra's algorithm or A* for optimal path finding
  - Plans in static map layer
- **Local Planner**: 
  - Dynamic Window Approach (DWA) for real-time obstacle avoidance
  - Trajectory optimization considering kinematic constraints
- **Costmap**: 
  - Multi-layer 2D grid: static map, obstacles, inflation
  - Real-time updates from sensor streams

### **Path Planning for Omnidirectional Robots**
- **Custom Configuration**: Adapted for mecanum wheel kinematics
- **Benefits**: Can utilize lateral motion for more efficient paths
- **Parameters**: Tuned for holonomic motion model

### **Coordinate Frames and TF Tree**

Understanding the transform tree (TF tree) is crucial for multi-sensor fusion and navigation:

```
map                      (World frame, static)
 └─→ odom                (Odometry frame, continuous)
      └─→ base_footprint (Robot ground projection)
           └─→ base_link (Robot center)
                ├─→ laser_frame      (RPLiDAR)
                ├─→ camera_link      (RealSense base)
                │    ├─→ camera_color_frame
                │    └─→ camera_depth_frame
                └─→ imu_link         (IMU sensor)
```

**Frame Descriptions:**

- **map**: Global fixed frame, origin of SLAM-built map
  - Set by SLAM system (gmapping, rtabmap)
  - Used for long-term planning

- **odom**: Locally accurate but drifts over time
  - Computed from wheel encoders (and/or visual odometry)
  - Smooth and continuous (no jumps)
  
- **base_link**: Robot's physical center
  - All robot sensors defined relative to this frame
  - Used for collision checking

- **Sensor frames**: Individual sensor coordinate systems
  - Transformations must be accurately calibrated
  - Critical for sensor fusion accuracy

**TF Tools:**

```bash
# Visualize TF tree
rosrun rqt_tf_tree rqt_tf_tree

# View transform between frames
rosrun tf tf_echo /map /base_link

# Check TF broadcast rates
rostopic hz /tf

# Debug TF issues
roswtf
```

**Common TF Issues and Solutions:**

| Issue | Symptom | Solution |
|-------|---------|----------|
| Missing transform | "Transform from X to Y does not exist" | Check if all drivers publishing transforms |
| Old transform | "Transform is X seconds old" | Increase cache time or fix timing issues |
| Frame ID mismatch | Navigation fails silently | Verify frame_id in sensor messages |
| Discontinuous jumps | Robot "teleports" in RViz | Check for multiple sources publishing same transform |

---

# 4 Tutorial

## 4.1 Preparing&Setup
```bash
# On server NUC
bash setup_from_scratch.sh # if this is a new setup
bash setup_environment_server # if opencv and ros is installed

sudo su
echo "server 127.127.1.0" >> /etc/ntp.conf
echo "fudge 127.127.1.0 stratum 5" >> /etc/ntp.conf
systemctl restart ntp.service

# add the following line to /etc/rc.local, before the "exit 0" line
bash /home/obt-sc/ros_workspace/SC0_ws/src/ocean_audio/script/server_bringup.sh

# On PC
bash setup_pc.sh # Only do this step if you didn't setup your pc envirnment at all. Manual setup is recommanded.
```

## 4.2 Compile & Build:
```bash
# Put the Mecanum_ros/src inside your workspace, for example: ~/ros_workspace/SC0_ws
cd ~/ros_workspace/SC0_ws/
catkin_make
```

## 4.3 ROS Remote Connection:

On PC (add following lines to ~/.bashrc):
```bash
export ROS_MASTER_URI=http://SERVER_IP_ADDRESS:11311
export ROS_HOSTNAME=PC_IP_ADDRESS
```

On Server (add following lines to ~/.bashrc):
```bash
export ROS_MASTER_URI=http://SERVER_IP_ADDRESS:11311
export ROS_HOSTNAME=SERVER_IP_ADDRESS
```

## 4.4 2D LIDAR SLAM

2D SLAM using laser rangefinder is the most mature and computationally efficient approach for indoor robot navigation.

**Technical Overview:**
- **Algorithm**: GMapping (Rao-Blackwellized Particle Filter SLAM)
- **Map Representation**: 2D occupancy grid (probabilistic)
- **Computational Complexity**: O(M·N·log(N)) where M = map cells, N = particles
- **Typical Performance**: 10-20 Hz update rate on NUC hardware

**Launch Sequence:**

```bash
# Terminal 1 (on NUC): Start the robot hardware interface and LIDAR
roslaunch sc_hw sc_hw.launch
roslaunch rplidar_ros rplidar.launch
```

### 4.4.1 Mapping:

**Procedure:**
1. Launch mapping algorithm
2. Visualize in RViz
3. Teleoperate robot to explore environment
4. Save map when exploration complete

```bash
# Terminal 2 (on NUC): Launch GMapping SLAM
roslaunch sc_2dnav gmapping.launch

# Terminal 3 (on PC): Launch visualization
rosrun rviz rviz -d `rospack find sc_2dnav`/rviz/HANDSFREE_Robot.rviz

# Terminal 4 (on PC): Control robot
roslaunch sc_hw mecanum_keyboard.launch

# After mapping is complete, save the map:
roscd sc_2dnav/map/
rosrun map_server map_saver -f your_map_name
```

**Mapping Tips:**
- Move slowly to avoid motion distortion
- Cover area systematically for complete coverage
- Close loops by revisiting previous areas to reduce drift
- Observe particle convergence in RViz (particles should cluster)

**Key Topics:**
- `/scan`: LaserScan data from RPLiDAR
- `/map`: Occupancy grid (published at 1 Hz)
- `/tf`: Transform tree (particularly `/odom` → `/base_link`)
	
<div align=center><img src="https://github.com/Merical/Mecanum_ros/blob/master/images/mapping.png" width=640 height=480></div>
	
### 4.4.2 Navigation:

**System Components:**
- **Localization**: AMCL (Adaptive Monte Carlo Localization)
- **Global Planner**: Dijkstra or A* on costmap
- **Local Planner**: DWA (Dynamic Window Approach)
- **Recovery Behaviors**: Rotation recovery, clearing costmap

**Launch Sequence:**

```bash
# Terminal 1 (on NUC): Start navigation with pre-built map
roslaunch sc_2dnav demo_move_base_amcl_server.launch map_name:=your_map_name

# Terminal 2 (on PC): Start visualization and goal setting interface
roslaunch sc_2dnav demo_move_base_amcl_client.launch
```

**Navigation Workflow:**
1. Robot localizes in pre-built map (give initial pose estimate in RViz)
2. Set navigation goal using "2D Nav Goal" button in RViz
3. Global planner computes optimal path
4. Local planner generates velocity commands while avoiding obstacles
5. Robot executes motion and monitors progress

**Parameters to Tune:**
- `xy_goal_tolerance`: Position tolerance (meters)
- `yaw_goal_tolerance`: Orientation tolerance (radians)
- `max_vel_x`, `max_vel_theta`: Velocity limits
- `path_distance_bias`: Preference for following global path
- `goal_distance_bias`: Preference for moving toward goal

<div align=center><img src="https://github.com/Merical/Mecanum_ros/blob/master/images/navigation.png" width=240 height=320></div>

**Debugging Navigation Issues:**
- **Poor localization**: Add more particles or reduce motion noise
- **Stuck in local minimum**: Increase oscillation timeout
- **Collisions**: Increase inflation radius in costmap
- **Slow navigation**: Increase velocity limits or reduce path resolution
	
## 4.5 RTAB-Map 3D Visual SLAM:

RTAB-Map (Real-Time Appearance-Based Mapping) provides appearance-based loop closure detection with 3D reconstruction capabilities.

**Technical Overview:**
- **Algorithm**: Graph SLAM with bag-of-words place recognition
- **Map Representation**: 3D point cloud + 2D occupancy grid projection
- **Loop Closure**: Visual similarity + geometric verification (RANSAC)
- **Memory Management**: Fixed memory size with intelligent forgetting mechanism
- **Backend Optimization**: g2o graph optimization
- **Feature Detection**: GFTT (Good Features To Track) or ORB

**System Requirements:**
- Sufficient lighting for visual features
- Textured environment (avoid feature-poor areas like white walls)
- Smooth motion to maintain feature tracking

**Hardware Setup:**

```bash
# Terminal 1 (on NUC): Start base hardware
roslaunch sc_hw sc_hw.launch

# Terminal 2 (on NUC): Start LiDAR (optional, for hybrid mapping)
roslaunch rplidar_ros rplidar.launch

# Terminal 3 (on NUC): Start RealSense with depth alignment
roslaunch realsense2_camera rs_camera.launch align_depth:=true
```

**Important**: The `align_depth:=true` parameter ensures depth and RGB images are aligned, critical for RGB-D SLAM.

### 4.5.1 Mapping:

**Mapping Mode: Building 3D Map from Scratch**

```bash
# Terminal 4 (on NUC): Start RTAB-Map in mapping mode
# --delete_db_on_start: Clear previous database and start fresh
roslaunch sc_2dnav demo_sc_rtab_mapping.launch args:="--delete_db_on_start"

# Terminal 5 (on PC): Visualization
roslaunch sc_2dnav demo_sc_rtab_rviz.launch

# Terminal 6 (on PC): Teleoperation
roslaunch sc_hw mecanum_keyboard.launch
```

**Mapping Strategy:**
1. **Initialization**: Move slowly for first 1-2 meters to establish initial map
2. **Exploration**: Systematic coverage with moderate speed (0.3-0.5 m/s)
3. **Loop Closure**: Revisit starting location from different angles to trigger loop closure
4. **Dense Coverage**: Move in grid pattern, maintain 60-70% feature overlap between frames
5. **Quality Check**: Monitor visualization - point cloud should appear consistent

**Key Topics:**
- `/rtabmap/cloud_map`: 3D point cloud of environment
- `/rtabmap/grid_map`: 2D occupancy grid (for navigation)
- `/rtabmap/mapData`: Map database with loop closure constraints
- `/rtabmap/info`: SLAM statistics (loop closures, timing, etc.)

**Monitoring SLAM Performance:**
- **Loop Closures**: Watch `/rtabmap/info` for loop closure events
- **Feature Count**: Ensure >100 features tracked per frame
- **Update Time**: Should be <100ms per frame for real-time performance
- **Database Size**: Monitor memory usage if mapping large areas

**Database Management:**
The map is saved in `~/.ros/rtabmap.db` by default. To specify custom location:
```bash
roslaunch sc_2dnav demo_sc_rtab_mapping.launch database_path:=/path/to/custom.db
```

### 4.5.2 Navigation (Localization Mode):

**Localization Mode: Use Pre-Built Map for Navigation**

```bash
# Terminal 4 (on NUC): Start RTAB-Map in localization mode
# Loads existing map and performs localization without updating map
roslaunch sc_2dnav demo_sc_rtab_mapping.launch localization:=true

# Terminal 5 (on PC): Visualization
roslaunch sc_2dnav demo_sc_rtab_rviz.launch
```

**Localization Process:**
1. **Initial Pose**: RTAB-Map attempts automatic relocalization using visual features
2. **Convergence**: May take several seconds as robot moves and observes environment
3. **Confidence**: Monitor `/rtabmap/localization_pose` covariance
4. **Navigation**: Once localized, can use standard navigation stack with generated 2D grid

**Advanced Features:**

**Multi-Session Mapping:**
To continue mapping in an existing database (without `--delete_db_on_start`):
```bash
roslaunch sc_2dnav demo_sc_rtab_mapping.launch
```

**Hybrid LiDAR-Visual SLAM:**
When both camera and LiDAR are active, RTAB-Map can:
- Use visual features for place recognition
- Use LiDAR scans for geometric constraints
- Improve robustness in challenging conditions

**Parameter Tuning:**

Important parameters in launch file:
- `frame_rate`: Image processing rate (default: 10 Hz for balance)
- `Mem/IncrementalMemory`: true for mapping, false for localization
- `Mem/InitWMWithAllNodes`: false for faster startup
- `RGBD/OptimizeMaxError`: Threshold for loop closure acceptance (default: 3.0)

**Troubleshooting:**
- **No loop closures detected**: Increase `Rtabmap/TimeThr` to allow more time for detection
- **False loop closures**: Decrease `Vis/MinInliers` threshold
- **Memory issues**: Reduce `Mem/STMSize` (short-term memory size)
- **Drift**: Ensure good feature tracking, move slower
	
<div align=center><img src="https://github.com/Merical/Mecanum_ros/blob/master/images/rtabmap.png" width=640 height=480></div>

**Export and Post-Processing:**

Export point cloud for external use:
```bash
# Export to PLY format
rosrun rtabmap_ros point_cloud_xyzrgb cloud:=/rtabmap/cloud_map _decimation:=4 _voxel_size:=0.01

# Export database to OctoMap format
rosrun rtabmap_ros rtabmap-export --format 8 --output mymap.ot ~/.ros/rtabmap.db
```

## 4.6 Audio:

```bash
roslaunch sc_hw sc_hw.launch
roslaunch ocean_audio ocean_audio.launch  (pc/nuc)   
recognize.py  (pc)
```

## 4.7 Visual Tracking and Following:

Visual object tracking enables the robot to follow a designated target, useful for human-robot interaction and dynamic object manipulation.

**Technical Overview:**
- **Algorithm**: CMT (Clustering-based Motion Tracking)
- **Features**: BRISK keypoints with online learning
- **Tracking Rate**: 15-30 Hz depending on object complexity
- **Control**: PID-based visual servoing
- **Depth Integration**: Uses RGB-D data for distance estimation

**Algorithm Characteristics:**
- **Initialization**: Manual bounding box selection in first frame
- **Online Adaptation**: Updates appearance model during tracking
- **Occlusion Handling**: Robust to partial occlusions
- **Scale Invariance**: Adapts to object size changes

**Launch Sequence:**

```bash
# Terminal 1 (on NUC): Start robot hardware
roslaunch sc_hw sc_hw.launch

# Terminal 2 (on NUC): Start RealSense with aligned depth
roslaunch realsense2_camera rs_camera.launch align_depth:=true

# Terminal 3 (on NUC or PC): Start tracking and control
roslaunch ocean_vision cmt_tracker_mecanum.launch
```

**Usage Workflow:**
1. **Initialization**: Click and drag to select object in video window
2. **Tracking**: Algorithm tracks selected object automatically
3. **Following**: Robot moves to maintain constant distance from object
4. **Termination**: Press 'q' to stop tracking

**Control Strategy:**

**Visual Servoing Loop:**
- **Lateral Control**: Proportional to object's horizontal offset from image center
- **Distance Control**: Maintains target distance using depth information
- **Angular Control**: Rotates to keep object centered

**PID Tuning Parameters:**
```python
# In cmt_tracker_mecanum.py or launch file:
Kp_x = 0.001  # Proportional gain for lateral control
Kp_theta = 0.005  # Proportional gain for rotation
target_distance = 1.0  # Desired distance to object (meters)
distance_tolerance = 0.1  # Acceptable distance error
```

**Key Topics:**
- `/camera/color/image_raw`: RGB image input
- `/camera/aligned_depth_to_color/image_raw`: Aligned depth image
- `/mobile_base/mobile_base_controller/cmd_vel`: Velocity commands output
- `/tracker/debug_image`: Annotated image with tracking bounding box

**Performance Considerations:**
- **Lighting**: Requires adequate and consistent lighting
- **Texture**: Works best with textured objects (avoid uniform colors)
- **Motion**: Moderate motion speed (<1 m/s relative velocity)
- **Distance**: Optimal tracking at 0.5-3 meters

**Applications:**
- Human-following robots
- Object manipulation and inspection
- Active vision experiments
- Human-robot interaction scenarios
	
<div align=center><img src="https://github.com/Merical/Mecanum_ros/blob/master/images/visual_tracking.png" width=320 height=240></div>

**Advanced Extensions:**

**Multi-Object Tracking:**
Modify code to track multiple objects simultaneously:
```python
# Initialize multiple trackers
trackers = [CMT() for _ in range(num_objects)]
```

**Learning-Based Detection:**
Replace CMT with deep learning detectors (YOLO, Faster R-CNN):
- Higher robustness to appearance changes
- Category-level detection (track all "persons")
- Requires GPU for real-time performance

**Predictive Control:**
Implement Kalman filter for motion prediction:
- Anticipate object motion
- Smooth control commands
- Reduce latency effects

## 4.8 Simulation (Gazebo):

Simulation provides a safe, reproducible environment for algorithm development and testing before deployment on physical hardware.

**Benefits of Simulation:**
- **Safety**: Test aggressive behaviors without hardware risk
- **Reproducibility**: Identical conditions across experiments
- **Rapid Iteration**: Faster than real-world testing
- **Scenario Testing**: Simulate rare or dangerous situations
- **Multi-Robot**: Easy to spawn multiple robots
- **Ground Truth**: Perfect knowledge of robot pose and environment

**Gazebo Features:**
- **Physics Simulation**: ODE physics engine for realistic dynamics
- **Sensor Simulation**: Simulated LiDAR, camera, IMU, encoders
- **World Models**: Pre-built environments or custom worlds
- **ROS Integration**: Seamless integration with ROS ecosystem

**Launch Simulation:**

```bash
# Terminal 1: Start Gazebo with robot model
roslaunch sc_gazebo demo_gazebo_sc0.launch

# Terminal 2: Start navigation stack (AMCL + move_base)
roslaunch sc_gazebo demo_move_base_amcl.launch
```

**Gazebo Launch Options:**

```bash
# Start with GUI (default)
roslaunch sc_gazebo demo_gazebo_sc0.launch gui:=true

# Headless mode (faster, for batch experiments)
roslaunch sc_gazebo demo_gazebo_sc0.launch gui:=false

# Custom world file
roslaunch sc_gazebo demo_gazebo_sc0.launch world_file:=/path/to/custom.world

# Spawn at specific pose
roslaunch sc_gazebo demo_gazebo_sc0.launch x:=2.0 y:=3.0 yaw:=1.57
```

**Simulated Sensors:**
- **LiDAR**: 360° laser scanner (matching RPLiDAR A2 specs)
- **RGB-D Camera**: Depth camera (matching RealSense D435i)
- **IMU**: 9-axis inertial measurement unit
- **Wheel Encoders**: High-resolution odometry
- **Ground Truth**: Perfect pose information for evaluation (not available on real robot)

**Navigation in Simulation:**

1. **Initial Localization**: Set initial pose estimate in RViz (2D Pose Estimate tool)
2. **Goal Setting**: Use "2D Nav Goal" to command robot
3. **Observation**: Watch robot plan and execute path
4. **Evaluation**: Compare actual trajectory to planned path

**Performance Metrics:**

Access ground truth for quantitative evaluation:
```bash
# Subscribe to ground truth pose
rostopic echo /gazebo/model_states

# Compare with estimated pose from SLAM
rostopic echo /robot_pose_ekf/odom_combined
```

**Custom World Creation:**

Create custom Gazebo worlds for specific scenarios:

```xml
<?xml version="1.0"?>
<sdf version="1.6">
  <world name="custom_world">
    <!-- Add ground plane -->
    <include>
      <uri>model://ground_plane</uri>
    </include>
    
    <!-- Add lighting -->
    <include>
      <uri>model://sun</uri>
    </include>
    
    <!-- Add obstacles -->
    <include>
      <uri>model://cafe_table</uri>
      <pose>2 0 0 0 0 0</pose>
    </include>
  </world>
</sdf>
```

**Sim-to-Real Transfer:**

Minimize sim-to-real gap:
1. **Sensor Noise**: Add realistic noise models to simulated sensors
2. **Dynamics**: Tune physics parameters to match real robot
3. **Delays**: Simulate communication and processing latencies
4. **Domain Randomization**: Vary environment properties during training

**Benchmarking Scenarios:**

Test algorithms in standardized scenarios:
- **Corridor Navigation**: Long, narrow passages
- **Cluttered Environment**: Dense obstacles requiring precise maneuvering
- **Dynamic Obstacles**: Moving objects (people, other robots)
- **Multi-Floor**: Stairs and elevators (for 3D navigation)
	
<div align=center><img src="https://github.com/Merical/Mecanum_ros/blob/master/images/simulation.png" width=320 height=240></div>

**Integration with ROS Tools:**

**Record Data:**
```bash
# Record all topics for offline analysis
rosbag record -a

# Record specific topics
rosbag record /scan /camera/color/image_raw /tf /odom
```

**Playback for Testing:**
```bash
# Play recorded data
rosbag play --clock my_simulation.bag

# Test SLAM with recorded data
roslaunch sc_2dnav gmapping.launch
```

**Automated Testing:**

Create launch files for automated experiments:
```python
# test_navigation.py
import rospy
from move_base_msgs.msg import MoveBaseGoal

goals = [(1, 0, 0), (1, 1, 0), (0, 1, 0), (0, 0, 0)]
for x, y, theta in goals:
    # Send goal and wait for completion
    # Log success rate, time, path length
```
	
## 4.9 SC_GUI
### Python3
Download weights and demo pictures from [Baidu Yun Link](https://pan.baidu.com/s/1T7QvCqoxyCtAedOI4d67PA)

Extract data/ and weights/ folder to the sc_gui_py3 dir.

```bash
# connect the robot wifi
roslaunch ocean_audio server_ros.launch (nuc)
cd sc_gui_py3 (pc)
python gui.py (pc)
```

## 4.10 Time Synchronization
```bash
sudo apt-get install -y ntpdate
sudo ntpdate -u SERVER_IP
```

## 4.11 Auto Driving Demo
Please refer to [Merical/AutoDrive](https://github.com/Merical/AutoDrive)
<div align=center><img src="https://github.com/Merical/AutoDrive/blob/master/Images/signdetection.png" width=640 height=480></div>

# 5 ROS Packakges
## 5.1 sc_hw
### 1) Overview
sc_hw is a ROS package for communication between the robot's embedded software system and the industrial computer. It includes serial communication, attitude calculation, sensor data reporting, command data transmission, odometry information publishing, robot control command reception, etc., establishing communication with the mobile platform through a polling strategy.

### 2) Sample Usage
To quickly establish the connection between the mobile platform and ROS system, use the following command to start the driver node and obtain odometry information:

```bash
roslaunch sc_hw sc_hw.launch
```

If you need to control the omnidirectional mobile platform using keyboard, use the following command:

```bash
roslaunch sc_hw mecanum_keyboard.launch
```

### 3) Nodes
#### sc_hw_node
ROS node for driving the omnidirectional intelligent mobile platform

##### Subscribed Topics
- **`/mobile_base/mobile_base_controller/cmd_vel`** ([geometry_msgs/Twist](http://docs.ros.org/api/geometry_msgs/html/msg/Twist.html))
  - Mobile platform motion velocity control topic, receives robot movement velocity commands

##### Published Topics
- **`/mobile_base/mobile_base_controller/odom`** ([nav_msgs/Odometry](http://docs.ros.org/api/nav_msgs/html/msg/Odometry.html))
  - Odometry information calculated by the mobile platform using encoders
- **`/handsfree/imu_data`** ([sensor_msgs/Imu](http://docs.ros.org/melodic/api/sensor_msgs/html/msg/Imu.html))
  - IMU attitude information obtained from the mobile platform's nine-axis sensor
- **`/handsfree/robot_state`** (sc_msgs)
  - Low-level status information reported by the mobile platform, including system time, battery level, etc.

##### Parameters
- **`~odom_linear_scale_correction`** (double, default: `1.0`)
  - Odometry linear movement error correction coefficient
- **`~odom_angle_scale_correction`** (double, default: `1.0`)
  - Odometry rotation error correction coefficient
- **`~serial_port`** (string, default: `"/dev/SCRobot"`)
  - Robot mobile platform USB binding port
- **`~base_mode`** (string, default: `"4omni-wheel"`)
  - Mobile platform mechanical structure type
- **`~with_arm`** (bool, default: `false`)
  - Whether equipped with robotic arm
- **`~controller_freq`** (double, default: `100`)
  - Mobile platform refresh rate

#### mecanum_teleop_key
ROS node for controlling the omnidirectional intelligent mobile platform using keyboard

##### Subscribed Topics
None

##### Published Topics
- **`/mobile_base/mobile_base_controller/cmd_vel`** ([geometry_msgs/Twist](http://docs.ros.org/api/geometry_msgs/html/msg/Twist.html))
  - Mobile platform motion velocity control topic, receives robot movement velocity commands

### 3) C/C++ Implementation Architecture

**Control Flow:**

```
main()
  │
  ├─→ Read configuration files
  ├─→ Initialize ROS node handle
  └─→ Enter mainloop()
       │
       └─→ HF_HW_ros::mainloop() [100 Hz control loop]
            │
            ├─→ HF_HW::checkHandsShake()
            │    └─→ Verify MCU communication
            │
            ├─→ HF_HW::UpdateCommand()
            │    └─→ Read data from MCU & Set robot actions
            │
            ├─→ HF_HW_ros::readBufferUpdate()
            │    ├─→ Parse MCU data packets
            │    ├─→ Update odometry
            │    ├─→ Publish /odom, /imu, /robot_state topics
            │    └─→ Broadcast TF transforms
            │
            ├─→ HF_HW_ros::writeBufferUpdate()
            │    ├─→ Subscribe to /cmd_vel
            │    ├─→ Convert twist to wheel velocities
            │    └─→ Send commands to MCU
            │
            └─→ controller_manager::update()
                 └─→ Update ROS control interfaces
```

**Data Flow Diagram:**

```
ROS Navigation Stack (/cmd_vel)
         │
         ↓
  [writeBufferUpdate]
         │
         ↓ (Serial USB)
  ┌──────────────┐
  │   MCU STM32  │
  │ - PID Control│
  │ - PWM Motors │
  │ - Encoders   │
  └──────────────┘
         │
         ↓ (Serial USB)
  [readBufferUpdate]
         │
         ├─→ /odom (Odometry)
         ├─→ /imu_data (IMU)
         ├─→ /robot_state (Battery, Status)
         └─→ /tf (odom→base_link)
```

**Key Classes:**

- `HF_HW_ros`: ROS wrapper class handling topic pub/sub
- `HF_HW`: Hardware abstraction layer for serial communication
- `Transport`: Low-level serial protocol implementation
- `TFProcessor`: Transform broadcasting and management

### 4)  MCU Communication Command Type

```
    SHAKING_HANDS          # Check communication handshake with MCU to ensure connection
    READ_SYSTEM_INFO       # Read MCU system info including runtime, battery level etc.
    SET_ROBOT_SPEED        # Set robot movement speed
    READ_ROBOT_SPEED       # Read robot movement speed
    READ_GLOBAL_COORDINATE # Read robot global coordinate information
    READ_IMU_FUSION_DATA   # Read IMU sensor data
    READ_INTF_MODE         # Read robot control authority
    SET_INTF_MODE          # Set robot control authority
    READ_MODULE_CONFIG     # Read robot module configuration
    READ_SONAR_DATA        # Read robot ultrasonic sensor data
    SET_SONAR_STATE        # Enable/disable robot ultrasonic sensors
    CLEAR_ODOMETER_DATA    # Clear robot coordinate information
```

## 5.2 rplidar
### 1) Overview
The LiDAR is mainly used for mapping, navigation, target tracking and other applications. It connects to the mobile platform via serial port. Dependencies: Communication between two computers is achieved through ROS, and LiDAR-related data is also transmitted to the host through ROS.
### 2) Usage

```bash
roslaunch rplidar_ros rplidar.launch
```

### 3) Nodes
#### rplidar Node
Drive rplidar_a2 and publish scan data

##### Subscribed Topics
None

##### Published Topics
- **`/scan`** ([sensor_msgs/LaserScan](http://docs.ros.org/kinetic/api/sensor_msgs/html/msg/LaserScan.html))
  - 2D laser scan data from the RPLidar

##### Parameters
- **`serial_port`** (string, default: `"/dev/rplidar"`)
  - Serial port name used in the system
- **`serial_baudrate`** (int, default: `115200`)
  - Serial port baud rate
- **`frame_id`** (string, default: `"laser_frame"`)
  - Coordinate system name of the device
- **`inverted`** (bool, default: `false`)
  - Indicates whether the lidar is mounted upside down
- **`angle_compensate`** (bool, default: `false`)
  - Whether angle compensation is needed
- **`scan_mode`** (string, default: `""`)
  - Scanning mode of the lidar

## 5.3 realsense2_camera
### 1) Overview
The Realsense camera is mainly used for 3D mapping, navigation and target tracking functions. The related information in Realsense is also published through topics.
Topics include depth images, RGB images, etc.
### 2) Usage

```bash
roslaunch realsense2_camera rs_camera.launch aligned_depth:=true
```

### 3) Nodes
#### realsense2_camera_nodelet
Drive realsense D435 and publish image data

##### Subscribed Topics
None

##### Published Topics

###### Color Camera
- **`/camera/color/camera_info`** ([sensor_msgs/CameraInfo](http://docs.ros.org/kinetic/api/sensor_msgs/html/msg/CameraInfo.html))
  - Camera calibration and metadata
- **`/camera/color/image_raw`** ([sensor_msgs/Image](http://docs.ros.org/jade/api/sensor_msgs/html/msg/Image.html))
  - Color image captured by the camera in RGB format

###### Depth Camera
- **`/camera/depth/camera_info`** ([sensor_msgs/CameraInfo](http://docs.ros.org/kinetic/api/sensor_msgs/html/msg/CameraInfo.html))
  - Camera calibration and metadata
- **`/camera/depth/image_raw`** ([sensor_msgs/Image](http://docs.ros.org/jade/api/sensor_msgs/html/msg/Image.html))
  - Depth image captured by the camera, pixel values are uint16 depth values
- **`/camera/aligned_depth_to_color/image_raw`** ([sensor_msgs/Image](http://docs.ros.org/jade/api/sensor_msgs/html/msg/Image.html))
  - Depth image aligned to RGB image perspective, pixel values are uint16 depth values

##### Parameters
- **`align_depth`** (bool, default: `false`)
  - Indicates whether to use aligned depth image

For more parameters and features, please see [realsense_ros](https://github.com/IntelRealSense/realsense-ros)

## 5.4 ocean_vision
### 1) Overview
Oceanbotech vision tracking ROS package, using CMT algorithm and PID control algorithm to implement intelligent mobile platform tracking functionality

### 2) Usage
```bash
roslaunch sc_hw sc_hw.launch
roslaunch realsense2_camera rs_camera.launch align_depth:=true
roslaunch ocean_vision cmt_tracker_mecanum_remote.launch
```

## 5.5 sc_2dnav
### 1) Overview
Oceanbotech 2D navigation package\
Using [gmapping](http://wiki.ros.org/gmapping/) algorithm for map building\
Using [move_base](http://wiki.ros.org/move_base/) and [amcl](http://wiki.ros.org/amcl) for real-time navigation\
Using [rtabmap](http://wiki.ros.org/rtabmap_ros) for 2D and 3D mapping and navigation

### 2) Usage


```bash
# Mapping:
roslaunch sc_2dnav gmapping.launch
rosrun rviz rviz -d `rospack find sc_2dnav`/rviz/HANDSFREE_Robot.rviz
roslaunch sc_hw mecanum_keyboard.launch
roscd sc_2dnav/map/
rosrun map_server map_saver -f your_map_name (on pc)

# Navigation:
roslaunch sc_2dnav demo_move_base_amcl.launch map_name:=your_map_name
```

---

# 6 Citation

If you use this platform in your research, please consider citing:

```bibtex
@misc{mecanum_ros2021,
  title={Mecanum ROS: An Open-Source Omnidirectional Mobile Robot Platform for SLAM and Computer Vision Research},
  author={Shenghao Li},
  year={2021},
  howpublished={\url{https://github.com/Merical/Mecanum_ros}},
  note={Accessed: [Date]}
}
```

# 7 Acknowledgments

This project integrates and builds upon numerous open-source projects and libraries:

## Core Dependencies
- **ROS (Robot Operating System)**: Foundation for modular robotics software
- **OpenCV**: Computer vision algorithms and image processing
- **PCL (Point Cloud Library)**: 3D point cloud processing
- **Eigen**: Linear algebra operations
- **g2o / Ceres**: Graph optimization and non-linear least squares

## SLAM and Navigation
- **gmapping**: Efficient 2D laser-based SLAM
- **rtabmap_ros**: RGB-D SLAM with loop closure
- **move_base**: Navigation stack for mobile robots
- **robot_localization**: Multi-sensor fusion framework

## Sensor Drivers
- **realsense-ros**: Intel RealSense camera driver
- **rplidar_ros**: Slamtec RPLiDAR driver

## Simulation
- **Gazebo**: Physics-based robot simulation
- **RViz**: 3D visualization for ROS

## Community Support
We thank the global robotics and open-source communities for their invaluable contributions, bug reports, and continuous support.

## Hardware Partners
- **Intel**: RealSense camera technology
- **Slamtec**: RPLiDAR sensor
- **Oceanbotech**: Hardware platform design and support