# NRMC2019
Repository for the Bison Robotics NASA Robotic Mining Competition 2019 Entry

Build status: [![Build Status](https://travis-ci.com/BisonRobotics/NRMC2019.svg?token=vbD9yxJbUfLNy2L7yUif&branch=master)](https://travis-ci.com/BisonRobotics/NRMC2019)

# Disclaimer
This repository is provided as is. There is little documentation, plenty of bugs, lots of bad ideas, and chunks of code that haven't been uploaded since I don't own them. At best the code in this repository can serve as an example for teams looking for an okay set of examples for an autonomous mining robot.

# Package structure
```
└── src
    ├── controls
    │   ├── competition - Controller for the competition
    │   ├── dig_control - Controller for the dig system
    │   └── waypoint_control - Controller for the drive system
    ├── description - URDF related code
    ├── hardware_layer - Meta package for drivers and hardware utilities
    │   ├── camera_calibration - Hacky opencv based camera calibrator
    │   ├── driver_access - Interface for the simulator
    │   ├── lpms_imu - Package for our IMU
    │   ├── motor_configs - VESC motor configs
    │   ├── sensor_access - Not included
    │   ├── stepper - Stepper driver
    │   └── vesc_access - Not included
    ├── launch
    ├── navigation - Meta package for navigation
    │   ├── localization - Not included
    │   ├── navigation_msgs - Custom ROS messages for navigation 
    │   └── tracker - AprilTag based absolute localization
    ├── simulation - Meta package for simulation
    │   ├── README.md
    │   ├── vrep_msgs - Custom ROS messages for VREP
    │   └── vrep_plugin - Plugin for VREP simulator
    └── utilities - Catch all for scripts and code
```

# Development Conventions
### GitHub
Every card on the sprint board should be tied to an issue. Any issue X that involves code 
development should have a corresponding branch named issue-X. In order to merge into master
a pull request has to be initiated and approved by one other developer. Generally a request 
shouldn't be merged if it doesn't have tests, its tests aren't passing, or the Travis-CI build 
fails. Once the pull request is merged, the corresponding issue should be closed, and
the branch corresponding to the request should be deleted. Deleting the branch will appear as 
a prompt on merge. A good way to automatically close the issue is to reference the issue using
a [keyword](https://help.github.com/articles/closing-issues-using-keywords/). For example I 
could reference issue 1 in my pull request by adding the text "closes #1". Try to update 
the project board when starting, reviewing, and finishing tasks.

## Style Guidelines
### ROS
See: http://wiki.ros.org/ROS/Patterns/Conventions  
And: http://wiki.ros.org/StyleGuide
### C++
See: http://wiki.ros.org/CppStyleGuide  
And: https://google.github.io/styleguide/cppguide.html
### Python
See: http://wiki.ros.org/PyStyleGuide

## Unit Testing
See: http://wiki.ros.org/Quality/Tutorials/UnitTesting

# Getting Started

## Setup your ssh keys
Follow these instructions: https://help.github.com/articles/connecting-to-github-with-ssh/

## Clone the repo
There are certain scripts that assume that you have the NRMC2019 repo installed in your home directory. It is recommended you don't try to clone the repo anywhere else.
```
cd ~
git clone git@github.com:BisonRobotics/NRMC2019.git
```

## Configure your OS
Much of the setup process was automated last year. Assuming you are running Ubuntu 16.04, you should be able to just run the following lines of code.
```
# Install ansible, restart your computer afterwards
sudo apt update
sudo apt install ansible

# Install dependencies
cd ~/NRMC2019
ansible-playbook -i "localhost," -c local src/utilities/ansible/dev_computer_playbook.yml
git lfs install
git lfs pull

# Build NRMC2019 and setup workspace
catkin_make
source devel/setup.bash

# Run unit tests
catkin_make run_tests
```
If this process doesn't work for you, the manual process is described [here](https://github.com/BisonRobotics/NRMC2019/wiki/Manual-Configuration)

## Setup an IDE
I highly recommend using the clion IDE available at https://www.jetbrains.com/clion/. Make sure you sign up for an education account so you don't have to pay for it. Otherwise if you're as hardcore as Dr. Ding, VIM works too. 

In order for clion to see the libraries available to ROS, you'll need to source your ROS install setup.bash and ROS workspace setup.bash (after a build) before launching clion from the same terminal

## Setup Workspace
Our workspace is the NRM2019 repository this README lives in. You can add some optional aliases that make launching stuff and working with ROS easier
```
echo "alias clion='~/clion/bin/clion.sh'"             >> ~/.bashrc
echo "alias ws='cd ~/NRMC2019'"                       >> ~/.bashrc
echo "alias wss='source ~/NRMC2019/devel/setup.bash'" >> ~/.bashrc
```

## Permissions issue
If you are having a permissions issue with git follow these [instructions](https://askubuntu.com/questions/858569/git-permissions-issue) 


## Understand how ROS works
Do the beginner tutorials: http://wiki.ros.org/ROS/Tutorials.

# Useful unit test commands for debugging
## Build and run single test suite
Tab complete is your friend here
```
catkin_make run_tests_wheel_control_gtest_test_WaypointControllerHelper2

# Build test for debugging 
catkin_make -DCMAKE_BUILD_TYPE=Debug test_WaypointControllerHelper2
```
## Debug unit tests
```
gdb ./devel/lib/wheel_control/test_WaypointControllerHelper2
```

# Speeding up compilation
```
# Disable compilation of GUIs and simulation
export ON_ROBOT=1 
# Disable message generation for languages other than cpp
export ROS_LANG_DISABLE=geneus:genlisp:gennodejs:genpy
```
