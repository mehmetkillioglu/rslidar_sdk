# **rslidar_sdk** 

Installation guide;

- Get the dependencies from below.

  ```sh
  sudo apt-get update
  sudo apt-get install -y libyaml-cpp-dev
  sudo apt-get install -y  libpcap-dev
  ```
  
- Clone this repository into Colcon workspace's src folder. Run submodule init and update commands to retrieve rs_driver repo as well.
```sh
git clone https://github.com/mehmetkillioglu/rslidar_sdk.git
cd rslidar_sdk
git submodule init
git submodule update
cd ..
```
- Clone [link](https://github.com/RoboSense-LiDAR/rslidar_msg) repository into Colco workspace's src folder
```sh
git clone https://github.com/RoboSense-LiDAR/rslidar_msg.git
```
- Go back to workspace and run the command "colcon build" 
- Run the demo by
```sh
ros2 launch rslidar_sdk start.py
```



# **v1.1.0**



---



### 1 Introduction

​	**rslidar_sdk** is the lidar driver software kit in Linux environment, which includes the driver core, ROS support, ROS2 support and Protobuf-UDP communication code. For users who want to use lidar driver through ROS, this software kit can be used directly. For users who want to do advanced development or integrate the lidar driver into their own projects, please refer to the lidar driver core. 



### 2 Download

- Method1 ------ Use git clone

Since rslidar_sdk project include the submodule --- rs_driver, user need to excute the following commands after git clone.

```sh
git clone XXXX.git
cd rslidar_sdk
git submodule init
git submodule update
```


### 3 Dependencies

- ROS (If use rslidar_sdk in ROS environment, ROS related libraries need to be installed, Ubuntu1604 - Install ROS kinetic desktop-full, Ubuntu1804 - install ROS melodic desktop-full)

  Installation： please refer to  http://wiki.ros.org

  **If you install ROS kinetic desktop-full or ROS melodic desktop-full，then the correspond PCL and Boost  will be installed at the same time. If will bring you a lot of convenience since you dont need to handle the version confliction. Thus, its highly recommanded to install ROS  desktop-full**

- ROS2(If use rslidar_sdk in ROS2 environment, ROS2 related libraries need to be installed, Ubuntu1604 - Not support yet, Ubuntu 1804 - Install ROS2 eloquent desktop)

  Installation: please refer to https://index.ros.org/doc/ros2/Installation/Eloquent/Linux-Install-Debians/

  **Note! Please avoid to install ROS and ROS2 in one computer at the same time! This may cause confliction! Also you may need to install PCL  manually.**

- Yaml >= v0.5.2 (Essential, if installed ROS desktop-full, this part can be ignored)

  Installation:

  ```sh
  sudo apt-get update
  sudo apt-get install -y libyaml-cpp-dev
  ```

- Protobuf (If use Protobuf related functions, Protobuf need to be installed)

  Installation :

  ```sh
  sudo apt-get install -y libprotobuf-dev protobuf-compiler
  ```

- pcap (Essential)

  Installation：

  ```sh
  sudo apt-get install -y  libpcap-dev
  ```



### 4 Compile & Run

We offer three ways to compile and run the driver

 - Compile directly

   Excute the commands below. In this way, user can also use ROS functions but need to start **roscore** manually before running the driver and need to start **rviz** manually to watch the pointcloud.

```sh
    cd rslidar_sdk
    mkdir build && cd build
    cmake .. && make -j4
    ./rslidar_sdk_node
```

- Compile with ROS-catkin

  - Open the *CMakeLists.txt* in the project，modify the line  on top of the file **set(COMPILE_METHOD ORIGINAL)** to **set(COMPILE_METHOD CATKIN)**

    ```cmake
    #=======================================
    # Compile setup (ORIGINAL,CATKIN,COLCON)
    #=======================================
    set(COMPILE_METHOD CATKIN)
    ```
  - Rename the file *package_ros1.xml*  in the rslidar_sdk to *package.xml*
    
  - Create a new folder as the workspace, and create a *src* folder in the workspace. Then put the rslidar_sdk project in the *src* folder. 
    
  - Return back to the workspace, excute the following command to compile and run. (if use .zsh, replace the 2nd command with *source devel/setup.zsh*)
  
    ```sh
    catkin_make
    source devel/setup.bash
    roslaunch rslidar_sdk start.launch 
    ```

  - Compile with ROS2-colcon

      - Open the *CMakeLists.txt* in the project，modify the line  on top of the file **set(COMPILE_METHOD ORIGINAL)** to **set(COMPILE_METHOD COLCON)**

        ```cmake
        #=======================================
        # Compile setup (ORIGINAL,CATKIN,COLCON)
        #=======================================
        set(COMPILE_METHOD COLCON)
        ```

    - Rename the file *package_ros2.xml*  in the rslidar_sdk to *package.xml*

    - Create a new folder as the workspace, and create a *src* folder in the workspace. Then put the rslidar_sdk project in the *src* folder. 

    - Download the packet definition project in ROS2 through [link](https://github.com/RoboSense-LiDAR/rslidar_msg), then put the project rslidar_msg in the *src* folder you just created.

    - Return back to the workspace, excute the following command to compile and run. (if use .zsh, replace the 2nd command with *source install/setup.zsh*)

      ```sh
      colcon build
      source install/setup.bash
      ros2 launch rslidar_sdk start.py 
      ```

    

### 5 File Structure

|- config												*Store all the configure files*

|- doc													*Store all the documents*

|- launch											  *Store the launch script for ROS and ROS2*

|- node												*Store the code of running node* 

|- rviz							    				   *Store the rviz configure file*

|- src							    					*Store the lidar driver core and functional codes*

|- CHANGELOG_CN.md           

|- CHANGELOG_EN.md

|- CMakeLists.txt

|- license

|- package_ros1.xml

|- package_ros2.xml

|- README.md



### 6 Introduction to parameters

**This part is very important, please read carefully. All the functions of this software kit will be reach by modifying parameters.**

[Intro to parameters](doc/intro/parameter_intro.md)



### 7 Quick start

**The followings are some quick guides to using some of the most common features of the rslidar_sdk, but the software kit are not limited to the following modes of operation. Users can use rslidar_sdk in their own way by modifying parameters.**

[Online connect lidar and send pointcloud through ROS](doc/howto/how_to_online_send_pointcloud_ros.md)

[Record rosbag & Offline decode rosbag](doc/howto/how_to_record_and_offline_decode_rosbag.md)

[Decode pcap bag and send pointcloud through ROS](doc/howto/how_to_offline_decode_pcap.md)

[How to use ROS2](doc/howto/how_to_use_ros2.md)



### 8 Advanced

[Intro to hiding parameters](doc/intro/hiding_parameters_intro.md)

[Use protobuf send & receive](doc/howto/how_to_use_protobuf_function.md)

[Multi-LiDARs](doc/howto/how_to_use_multi_lidars.md)
