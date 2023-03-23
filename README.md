# rosgazebo
### Intruction for "~ Ubuntu 20.04 + ROS Noetic" --> Gazebo Classic
#### Update Whole Process from [Eungchang's git](https://github.com/engcang/mavros-gazebo-application) (2023.3.1) 
### Installation 
+ Download SITL and MAVROS 
~~~shell
    $ sudo apt-get install ros-<distro>-mavros ros-<distro>-mavros-extras
    $ wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
    $ chmod +x install_geographiclib_datasets.sh
    $ sudo ./install_geographiclib_datasets.sh
    
    $ git clone https://github.com/PX4/PX4-Autopilot.git
    $ cd PX4-Autopilot
    $ git reset --hard 6823cbc   //previous firmware
    $ git submodule update --init --recursive
    $ source PX4-Autopilot/Tools/setup/ubuntu.sh --no-sim-tools
~~~
+ Environment Setup
~~~shell
    $ cd ~/PX4-Autopilot
    $ sudo apt install libgstreamer-plugins-base1.0-dev ros-<distro>-gazebo-plugins
    $ DONT_RUN=1 make px4_sitl_default gazebo
    
    $ source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/px4_sitl_default
    $ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)
    $ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)/Tools/sitl_gazebo
    $ roslaunch px4 mavros_posix_sitl.launch
~~~
+ In the bashrc,
~~~shell
$ echo "export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/home/$(whoami)/PX4-Autopilot/Tools/sitl_gazebo/models" >> ~/.bashrc
$ echo "export GAZEBO_PLUGIN_PATH=/home/$(whoami)/PX4-Autopilot/build/px4_sitl_default/build_gazebo" >> ~/.bashrc
$ echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/$(whoami)/PX4-Autopilot/build/px4_sitl_default/build_gazebo" >> ~/.bashrc
$ echo "export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/$(whoami)/PX4-Autopilot:/home/$(whoami)/PX4-Autopilot/Tools/sitl_gazebo" >> ~/.bashrc
$ . ~/.bashrc
~~~

+ QGroundControl Install
https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html

+ Edit Launch file for SITL
~~~shell
        <arg name="fcu_url" default="udp://:14540@localhost:14557" />
~~~

+ Run PX4-Autopilot
~~~shell
    $ roslaunch px4 mavros_posix_sitl.launch   //default setup
~~~

+ Run mavros + SITL separately,
~~~shell
    $ roslaunch mavros px4_sim.launch // make launch file for sim.
    $ make px4_sitl gazebo
~~~

+ GPS-based operation
~~~shell
    run SITL + QGC (or custom GCS)
~~~

+ LiDAR SLAM-based operation
~~~shell
    - Drone sdf modeling
    : velodyne sdf (velo2cam pkg)
    : assembling (link tree)
    : inertia edit (+mass)
    : libvlp16.so plugin (path setup)
    - World modeling
    - QGC setting
    : params (ekf2_aid_mask, ...)
    - How to run A-LOAM
    : A-LOAM opencv4 error -> code editting
    : launch file description
    - Test
~~~


+ Kill gazebo
~~~shell
$ killall gzserver
$ killall gzclient
~~~
