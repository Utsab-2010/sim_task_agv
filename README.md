## STATUS: Completed
### Documentation: [Notion](https://www.notion.so/ROS-Tasks-2-8af6dea9be8e4f8fbe3898493c988c86?pvs=4)


## Instructions for AGV SIM TASK 

### Catkin Workspace
Create a catkin workspace through the following commands
```
mkdir –p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
cd ~/catkin_ws/
catkin_make
source ~/catkin_ws/devel/setup.bash
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```
To ensure your workspace is properly set up, check for the ROS_PACKAGE_PATH using the following command. You must see the current directory similar to 
```
echo $ROS_PACKAGE_PATH
```
> /home/<username>/catkin_ws/src:/opt/ros/noetic/share:/opt/ros/noetic/stacks
### Cloning the current repository
All the executables and folders are to be placed in src/ and catkin_make every time you modify the src/ folder.
Hence, navigate to the src/ folder and clone the required repositories & dependencies as follows:
```
cd ~/catkin_ws/src
git clone https://github.com/tharun-selvam/sim_task_agv.git
```
### Dependencies
```
git clone https://github.com/nilseuropa/realsense_ros_gazebo.git
```
```
git clone https://github.com/issaiass/realsense2_description.git
```
```
sudo apt-get install ros-noetic-teleop-twist-keyboard ros-noetic-urdf ros-noetic-xacro ros-noetic-rqt-image-view  ros-noetic-robot-state-publisher ros-noetic-joint-state-publisher-gui
```
Before moving on to launching the setup, navigate to the root of the workspace and make the workspace
```
cd ~/catkin_ws
catkin_make
```
Use the commands in the following section to launch the bot.
### Running Gazebo Simulations
In terminal 1,
``` 
roslaunch tortoisebotpromax_gazebo tortoisebotpromax_playground.launch
```
### Running RViz Visualization
Open another terminal (terminal 2)
``` 
roslaunch tortoisebotpromax_description display.launch
```
You may change the Rviz configuration by adding topics for generating maps, visualization purposes, etc.,
### Teleoperation of the bot
In terminal 3
```
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```
and follow the instructions in the terminal for teleoperation. 
Note: Use keyboard commands while being in the terminal for publishing the control commands.  

You may subscribe to the needed sensor feedback and publish your control commands through corresponding rostopics via subscribers and publishers.
To list down all rostopics, use
```
rostopic list
```
# Getting the TortoisebotProMax Running
Here's a quick overview of the launch files that need to be initiated to get the robot running in autonomous/manual mode:
1. **bringup.launch**: Initiates the necessary hardware nodes and state publishers on the robot.
 Launching this script will get you:
- Teleop control
- Lidar visualization
- Wheel odometry
- Imu data
- State publishers
- Internal PID control
Here's the command line:
```
roslaunch tortoisebotpromax_firmware bringup.launch pid:=true
```
(Note: You can turn PID off simply by changing the argument to pid:=false)

2. **server_bringup.launch**: Initiates necessary localization tools for autonomous navigation
Launching this script will get you:
- Cartographer Node Initialization
- Laser-based odometry
Here's the command line:
```
roslaunch tortoisebotpromax_firmware server_bringup.launch
```
3. **tortoisebotpromax_navigation.launch**: Initiates the autonomous navigation for exploration as well as localization modes
Launching this script will get you:
- Cartographer Based Mapping/Localization
- Move base navigation tools
Here's the command line:
```
roslaunch tortoisebotpromax_navigation tortoisebotpromax_navigation.launch exploration:=true
```
Note: 
- exploration:true enables the robot to move in SLAM mode. It can autonomously navigate in the environment while creating the map for the same.
- exploration:=false mode sets the robot in localization mode. It spawns itself in the map you provide and navigates within the space.
- When in localization mode, you can provide the map file by:
```
roslaunch tortoisebotpromax_navigation tortoisebotpromax_navigation.launch exploration:=false map_file:/"location of your map file"
```

## Task
After launching the above setup, 
1. You will need to visualize the lidar point cloud in Rviz. (Take a screenshot of Rviz as proof) 
2. Teleoperate, scan the AruCo markers and store the corresponding waypoint information and robot's pose.
3. Generate an occupancy grid-based map ( You may use established algorithms by mentioning) while teleoperating with the bot.
4. Finally, drive the robot autonomously through a sequence of waypoints stored.


### General Useful references
1. ROS Tutorials -[http://wiki.ros.org/ROS/Tutorials](http://wiki.ros.org/ROS/Tutorials)
2. Rviz User guide -[http://wiki.ros.org/rviz/UserGuide](http://wiki.ros.org/rviz/UserGuide)

### Some useful tips
- Try reading on launch files, ros params, urdf files to get an idea of the code base
- Learning about urdf files also exposes you to learn about Gazebo and Rviz which really helps
- As you might have realised by now, there aren't many video tutorials on ROS. So you have to learn by going through the documentation
- Google WHATEVER error you face and go through every link and comment on stackoverflow 

