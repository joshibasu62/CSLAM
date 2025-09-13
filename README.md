# CSLAM
### Make your workspace
mkdir -p ~/fast_lio2_ws/src  
cd ~/fast_lio2_ws/src  

### Install livox_ros_driver(it should be inside your src folder of workspace)
git clone https://github.com/atinfinity/livox_ros_driver2.git ws_livox/src/livox_ros_driver2 -b jazzy 

### livox-sdk2
'git clone https://github.com/Livox-SDK/Livox-SDK2.git'  
cd Livox-SDK2  
mkdir build  
cd build  
cmake .. && make -j  
sudo make install  

### After the build fails add following header
'#include <cstdint>'   
Inside  
~/fast_lio2_ws/src/Livox-SDK2/sdk_core/comm/define.h  
~/fast_lio2_ws/src/Livox-SDK2/sdk_core/logger_handler/file_manager.h  

### then

cd ~/fast_lio2_ws/src/Livox-SDK2  
rm -rf build  
mkdir build && cd build  
cmake .. -DCMAKE_CXX_STANDARD=14  
make -j$(nproc)  
sudo make install  

## Build the Livox ROS Driver 2 for ros2 jazzy
cd ..  
cd ws_livox  
rosdep install -y -i --from-paths src/livox_ros_driver2  
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release -Wno-dev  

### after building the livox_driver you need to source it before building Fast_lio_ros2 inside your bashrc.
source ~/fast_lio2_ws/src/ws_livox/install/setup.bash

# Ericsii's FAST_LIO ROS2 wrapper
git clone https://github.com/Ericsii/FAST_LIO_ROS2.git  
cd FAST_LIO_ROS2  
git submodule update --init --recursive  
cd ..  
rosdep install --from-paths src --ignore-src -y  
colcon build --symlink-install  
