# RM Armor Tracker

This project aims to propose an **easy-to-use** set of detector and tracker.  
The feature code contains a lot of comments, and the **loosely** coupled framework allows for unit testing.  
The overall deployment scheme is gradual and can be used as a **beginner**'s practice project.  
____

## Introduce

- __Recommended environment__

Based on the chenjun open-source rm_vision framework,  
The hardware of this project depends on **RDK_X5** and **Daheng industrial camera**, and the software depends on **Ubuntu2204** and **ROS2humble**

- __Changelog__

2025-1-8：Upload Basic functions  
<pre>
.  
└── src  
  └── rm_armor_tracker  
    ├── rm_camera_driver  
    └── rm_utils  
</pre>
2025-2-6：Add Detector  
<pre>
.  
└── src  
  └── rm_armor_tracker  
    ├── rm_interfaces  
    ├── rm_camera_driver  
    ├── armor_detector  
    └── rm_utils  
</pre>
____

## Installation Instructions

### Get source code  

- Create your workspace

  ```shell
  mkdir -p rm_ws/src
  ```  

  ```shell
  cd rm_ws
  ```  

- Initialize the workspace  

  ```shell
  colcon build
  ```  

  ```shell
  cd src
  ```  

- Fetch source code  

  ```shell
  git clone https://github.com/tianbot/rm_armor_tracker.git
  ```

[rm_armor_tracker-dev
](https://github.com/tianbot/rm_armor_tracker/tree/dev)
____

### Dependencies

Update the software package index  

  ```shell
  sudo apt-get update
  ```  

#### Common component

- __camera_info_manager__  

>It provides a C++ class used by many camera drivers to manage the camera calibration data required by the ROS image pipeline.  

  ```shell
  sudo apt-get install ros-humble-camera_info_manager
  ```  

- __image_transport__  

>image_transport should always be used to subscribe to and publish images. It provides transparent.  

  ```shell
  sudo apt-get install ros-humble-image_transport
  ```

- __fmt__

>an open-source formatting library providing a fast and safe alternative to C stdio and C++ iostreams.

  ```shell
  sudo apt-get install libfmt-dev
  ```

- __spdlog__

>Fast C++ logging library

  ```shell
  sudo apt install  libspdlog-dev
  ```

- __Qt__

>Qt is a cross-platform application development framework for desktop, embedded and mobile.

  ```shell
  sudo apt install    qtdeclarative5-dev qt5-qmake libqglviewer-dev-qt5
  ```

#### Math tools

- __eigen__

>Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms.

  ```shell
  sudo apt install libeigen3-dev
  ```

- __suitesparse__

> SuiteSparse is a set of sparse-matrix-related packages

  ```shell
  sudo apt install   libsuitesparse-dev
  ```

- __Ceres__  

>Ceres Solver 1 is an open source C++ library for modeling and solving large, complicated optimization problems.  

  ```shell
  git clone --recurse-submodules https://github.com/ceres-solver/ceres-solver.git
  ```  

  ```shell
  cd ceres-solver-2.2.0
  ```  

  ```shell
  sudo gedit ./CMakeLists.txt
  ```

  find_package(Eigen3 3.3 REQUIRED)  >> find_package(Eigen3  REQUIRED)  
  
  ```shell
  mkdir build && cd build
  ```  

  ```shell
  cmake ..
  ```

  ```shell
  make -j
  ```  

  ```shell
  sudo make install  
  ```  

- __Sophus__  

>Sophus is a C++ implementation of Lie groups commonly used for 2d and 3d geometric problems

  ```shell
  git clone https://github.com/strasdat/Sophus
  ```  

  ```shell
  cd Sophus-main
  ```  

  ```shell
  sudo gedit ./CMakeLists.txt
  ```

  cmake_minimum_required(VERSION 3.24)  >> cmake_minimum_required(VERSION 3.16)

  ```shell
  mkdir build && cd build
  ```  

  ```shell
  cmake ..
  ```

  ```shell
  make -j
  ```  

  ```shell
  sudo make install  
  ```

- __G2O__

>g2o is an open-source C++ framework for optimizing graph-based nonlinear error functions.

  ```shell
  git clone https://github.com/RainerKuemmerle/g2o
  ```  

  ```shell
  cd g2o
  ```  

  ```shell
  mkdir build && cd build
  ```  

  ```shell
  cmake ..
  ```

  ```shell
  make -j
  ```  

  ```shell
  sudo make install  
  ```  

<!-- ____ -->
<!-- ### Cmake  
add External library in CMakeList.txt  
<pre>
find_package(ament_cmake_auto REQUIRED)
find_package(camera_info_manager REQUIRED)
find_package(image_transport REQUIRED)
</pre> -->
____

## Compilation  

- Build project  

  ```shell  
  cd ~/rm_armor_tracker
  ```  

- Compile the feature package  

  ```shell
  colcon build  --packages-select rm_utils
  ```  

  ```shell
  colcon build  --packages-select rm_interfaces
  ```  

  ```shell
  colcon build  --packages-select rm_camera_driver
  ```  

  ```shell
  colcon build  --packages-select armor_detector
  ```  

- Add environment variables  

  ```shell
  source install/setup.bash
  ```  

____

## Launch Project

### Start Camera Node  

```shell
ros2 run rm_camera_driver rm_camera_node
```  

__Topic__: /image_raw  

### TF Static Publisher

```shell
ros2 run tf2_ros static_transform_publisher --frame-id odom --child-frame-id camera_optical_frame --x 0.5 --y 0.0 --z 0.0 --roll 0.0 --pitch 0.0 --yaw 0.0
```

### Start Detector Node  

```shell
ros2 run armor_detector armor_detector_node
```  
__Topic__: /armor_detector/result_img  

###

### Visualization  

- launch rviz2
`rviz2`  

- Add by topic
`image_raw`  
`binary img`  
`result img`  

- QoS seting
`Reliable >> Best Effort`  

____

## result  

##### origin img

![origin](./doc/origin.png)

##### binary img

![binary](./doc/binary.png)

##### result img

![result](./doc/result.png)

##### fps:203(i7 10800H)

![fps](./doc/fps.png)
____

## Topics  

/armor_detector/armors  
/armor_detector/binary_img  
/armor_detector/debug_armors  
/armor_detector/debug_lights  
/armor_detector/heartbeat  
/armor_detector/marker  
/armor_detector/number_img  
/armor_detector/result_img  
/camera_driver/heartbeat  
/camera_info  
/clicked_point  
/goal_pose  
/image_raw  
/initialpose  
/parameter_events  
/rosout  
/tf  
/tf_static  

____

## To Do List

- [x] Camera Node 25.1.8
- [x] Detector Node 25.2.6
- [ ] Tracker Node 25.2.10
- [ ] Unit Testing 25.2.15

____

## License  

The tracker_node is prietary. Packages like rm_bringup、rm_camera_driver are under __MIT__ license.  
Galaxy SDK is under commercial license.  

## Contact Us

Email
><lenardo_smile@outlook.com>

Github
><https://github.com/tianbot>

Technical Support
>
>- RDK: @wunuo
<https://github.com/wunuo1/rm_armor_detection/tree/main>
>- Galaxy: @jerry
