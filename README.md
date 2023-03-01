# rosgazebo
### Intruction for "~ Ubuntu 20.04 + ROS Noetic" --> Gazebo Classic
## Update Whole Process from [Eungchang's git](https://github.com/engcang/mavros-gazebo-application) (2023.3.1) 
### Installation 
+ Download SITL and MAVROS 
~~~shell
    $ sudo apt-get install ros-<distro>-mavros ros-<distro>-mavros-extras
    $ wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
    $ chmod +x install_geographiclib_datasets.sh
    $ sudo ./install_geographiclib_datasets.sh
    
    $ git clone https://github.com/PX4/PX4-Autopilot.git
    $ cd PX4-Autopilot
    $ git submodule update --init --recursive
    $ source PX4-Autopilot/Tools/setup/ubuntu.sh --no-sim-tools
~~~
+ Environment Setup
~~~shell
    $ cd ~/PX4-Autopilot
    $ sudo apt install libgstreamer-plugins-base1.0-dev ros-<distro>-gazebo-plugins
    $ DONT_RUN=1 make px4_sitl_default gazebo
    
    $ source Tools/simulation/gazebo-classic/setup_gazebo.bash $(pwd) $(pwd)/build/px4_sitl_default
    $ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)
    $ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)/Tools/simulation/gazebo-classic/sitl_gazebo-classic
    $ roslaunch px4 mavros_posix_sitl.launch
~~~
+ In the bashrc,
~~~shell
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/home/$(whoami)/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/models
export GAZEBO_PLUGIN_PATH=/home/$(whoami)/PX4-Autopilot/build/px4_sitl_default/build_gazebo
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/$(whoami)/PX4-Autopilot/build/px4_sitl_default/build_gazebo
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/$(whoami)/PX4-Autopilot:/home/$(whoami)/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic
~~~
