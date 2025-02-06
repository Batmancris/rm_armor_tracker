# RM Armor Tracker
Based on the chenjun open-source rm_vision framework,   
This project relies on __RDK_X5__ and __Daheng MER-139 industrial camera__  
Other hardware solutions may also applicable
____
## Changelog
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
> - git clone https://github.com/tianbot/rm_armor_tracker.git

[rm_armor_tracker-dev
](https://github.com/tianbot/rm_armor_tracker/tree/dev)
____
### External library  

Update the software package index
#### Apt installation  
>- sudo apt-get update  

__camera_info_manager__  
>- sudo apt-get inatsll ros2-humble-camera_info_manager  

__Transport__  
>- sudo apt-get inatsll ros2-humble-image_transport  

__fmt__
>- sudo apt-get inatsll libfmt-dev

__Eigen__
>- sudo apt install libeigen3-dev libspdlog-dev libsuitesparse-dev qtdeclarative5-dev qt5-qmake libqglviewer-dev-qt5

#### Source code installation
__Ceres__  
>- git clone --recurse-submodules https://github.com/ceres-solver/ceres-solver.git
>- cd ceres-solver  
>- mkdir build  
>- cd build  
>- cmake ..
>- make -j
>- sudo make instal

__Sophus__
>- git clone https://github.com/strasdat/Sophus  
>- cd Sophus  
>- mkdir build && cd build  
>- cmake ..  
>- make -j  
>- sudo make install  

__G2O__
>- git clone https://github.com/RainerKuemmerle/g2o
>- cd g2o
>- mkdir build && cd build
>- cmake ..
>- make -j
>- sudo make install

____
### Cmake  
add External library in CMakeList.txt  
<pre>
find_package(ament_cmake_auto REQUIRED)
find_package(camera_info_manager REQUIRED)
find_package(image_transport REQUIRED)
</pre>
____
## Usage Instructions  
Build project  
>- cd ~/rm_armor_tracke

Compile the feature package  
>- colcon build  --packages-select rm_utils
>- colcon build  --packages-select rm_interfaces
>- colcon build  --packages-select rm_camera_driver
>- colcon build  --packages-select armor_detector

Add environment variables  
>- source install/setup.bash  

Start camera node  
____
## Launch Project
### Start Camera Node  
>- ros2 run rm_camera_driver rm_camera_node  

### TF Static Publisher
>- ros2 run tf2_ros static_transform_publisher --frame-id odom --child-frame-id camera_optical_frame --x 0.5 --y 0.0 --z 0.0 --roll 0.0 --pitch 0.0 --yaw 0.0

### Start Detector Node  
>- ros2 run armor_detector armor_detector_node  

### 
### Visualization  
>- rviz2

Add by topic
>- image_raw  
>- binary img  
>- result img

Qos seting
> Reliable >> Best Effort
____
## result  
##### origin img
![origin](./doc/origin.png)
##### binary img
![binary](./doc/binary.png)
##### result img
![result](./doc/result.png)
##### fps:230(i7 10800H)
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
- [x] Camera Node
- [x] Detector Node
- [ ] Tracker Node
____ 
## License  
The tracker_node is prietary. Packages like rm_bringup、rm_camera_driver are under __MIT__ license.  
Galaxy SDK is under commercial license.  
## Contact Us
Email
>lenardo_smile@outlook.com

Web
>www.tianbot.com

Technical Support
>- RDK: @wunuo
https://github.com/ultralytics/ultralytics/tree/v8.2.103
>- Galaxy: @jerry