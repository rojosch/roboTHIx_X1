SETUP PROCEDURE

1. Install the latest Linux distribution named UBUNTU on your computer (select LTS (= Long Term Support))
	Download:			https://ubuntu.com/
	Official installation guide: 	https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview
	
	Furthermore see my personal installation guide (see "Guides")

3. Install ROS:
	Note: To the day of this edit, the latest Version of ROS 1 is "noetic". We recommend to use this version.
	
	1. Open Terminal (Ctrl + Alt + T):
	2. Follow the official Installation Guide (1.1. to 1.6.):	http://wiki.ros.org/noetic/Installation/Ubuntu
	Note: We recommend you to install the "Desktop-Full Installation" (It also includes handy tutorial elements to get familiar with ROS)
	3. Under "1.5. Environment Setup", select "Bash", and type 
		echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
		source ~/.bashrc
	   as described. 
	Note: This script will now be so-called "sourced" everytime a new terminal/shell is launched. Your change will be added at the bottom of that script. You can take a look at it by typing
	   	gedit .bashrc
	4.  1.6. : Done so far
	

4. Create a catkin workspace:
	Official Guide: http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment
	
	1. Go to "3. Create a ROS Workspace" and proceed with catkin
	2. When finished, set up an extended toolset for catkin, which allows easier handling and outputs more precise error messages:
		
		Installing on Ubuntu with apt-get

		First you must have the ROS repositories which contain the .deb for catkin_tools:
		Type
			$ sudo sh \
			    -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" \
				> /etc/apt/sources.list.d/ros-latest.list'
			$ wget http://packages.ros.org/ros.key -O - | sudo apt-key add -

		Once you have added that repository, run these commands to install catkin_tools:
		Type
			$ sudo apt-get update
			$ sudo apt-get install python3-catkin-tools

		
		Instead of "catkin_make", you can now use 
			catkin build 
		to compile and build your workspace.
	
	
	3. Make sure you are in home directory. Open your .bashrc by typing
		gedit .bashrc	
	   Add a new line to the end of your .bashrc
		$ source ~/catkin_ws/devel/setup.bash
	   and save and exit


5. Install these external depended packages:
	# As source (= installation to catkin workspace as source code):
	  * [`ds4_driver`](https://github.com/naoki-mizuno/ds4_driver) and 
	  * [`ds4_drv`](https://github.com/naoki-mizuno/ds4_drv) to use a PlayStation 4 Controller connected to PC via Bluetooth 
	  * [`phidgets_drivers`](https://github.com/ros-drivers/phidgets_drivers/tree/noetic) to use the Phidget motor drivers (and other sensors in the future). When you build your workspace, in the first run it's normal that build errors will be displayed. Just source your ROS folders and build it again. The original phidgets_drivers package has been modified. How you replace the sub-folder **phidgets_drivers** with the **phidgets_drivers** folder from this package will be explained in a later step of this explanation. 
	  * [`sick_safetyscanner`](http://wiki.ros.org/sick_safetyscanners) to use the Sick nanoScan 3 in the front of the robot. Please install from source, not from binaries!
	  * [`hector_slam`](http://wiki.ros.org/hector_slam) 
	  
	# As binaries (as precompiled code to system folder):
	  * ['twist_mux']() by sudo apt-get install ros-noetic-twist-mux. Multiplexes and prioritizes multiple sources of **cmd_vel** to one output
	  * ['rosserial_arduino']() by sudo apt-get install ros-noetic-rosserial. ROSserial interface nodes, e.g. for interfacing an Arduino to control the LED stripes as well as interfacing the Geiger-Detector
	  * [`ublox`]() by sudo apt-get install ros-noetic-twist-mux. U-Blox GNNS IMU sensor

	# Installation
	Pull/Fork the Project Repositories:
	Link to Git Repository:		https://github.com/rojosch/roboTHIx_X1	TBD
	
	To download the repository, type:
		```console
		$ cd ~/catkin_ws/src
		$ git clone https://github.com/rojosch/roboTHIx_X1.git
		```
	You will now find the files in your src directory
	
	Note: We recommend to use GIT for working with code as a team. Therefore it is necessary to get familiar with the capabilities of GIT:
		More Information: 	https://github.com
	
		
	To download and install the robot localization package with our changes:
	```console
	$ cd ~/catkin_ws/src
	$ git clone https://github.com/MDkontroller/robot_localization.git
	```
	
	Compile and source the package
	```console
	$ cd ~/catkin_ws
	$ catkin build
	$ source /opt/ros/noetic/setup.bash
	$ source ~/catkin_ws/devel/setup.bash 
	```

	# Replace Phidgets Files
	To replace the original phidgets_drivers sub-package with the [modded](https://github.com/roboTHIx/thi_modded_phidgets_drivers) one
	```console
	$ git clone https://github.com/roboTHIx/thi_modded_phidgets_drivers
	$ cd thi_modded_phidgets_drivers
	$ chmod +x replace_phidgets_files.sh
	$ ./replace_phidgets_files.sh
	```
	After this, you will be asked if you really want to replace the original phidgets_drivers files. If you answer with "Y" or "y", the files in the ~/catkin_ws/src/phidgets_drivers folder will be replaced.
	Build your workspace again
	```console
	$ cd ~/catkin_ws
	$ catkin build
	```

6. Operate the Robot:
	to take the Robot into service, first
		- connect all USB cables to your laptop (aka Robot PC). If using an USB Hub, check that it is switched on.
		- connect DS4-Controller to your laptop (aka Robot PC) via Bluetooth 
		- connect the LAN cable to your laptop
		- turn on the Robot mains
		- wait for the Router to start up
		- Set Emergency Switch to NOT ENGAGED
	
	open Terminal and type:
		roslaunch robothix_x1_base full_bringup.launch
	
	You can now operate the robot manually via Controller (see "Gamepad_Assignment").
	
	For automatic navigation reliant on GPS, make sure the GPS sensor receives a propper signal (-> usually only works outside)
	WARNING: 	The Robot Operator (= person holding the controller) takes resposility for the Robot at any given moment! 
			An Obstacle detection is NOT implemented. To avoid casualties, the opertor needs to take control via controller to overrule the autonavigation temporarily. 
	
	To start the GPS autonavigation, open a new Terminal and type 
		rosrun robothix_x1_navigation gotogoalGPS
	Select, if you prefer to work with a north-aligned x/y coordinate system (y = North, x = East), or pure GNNS coordinate entry, and enter your target.
	
	To stop programs, type ( Ctrl + C) in terminal.
	
	Take care and have fun!

