## To build docker run:

### I'm not sure if you have to do this but why not
xhost + 

sudo docker build -t name_of_thing .

sudo docker run -it --rm \
	-p 2233:2233 \
	--env="DISPLAY=$DISPLAY" \
	--env="XDG_SESSION_TYPE=$XDG_SESSION_TYPE" \
	--env="QT_X11_NO_MITSHM=1" \
	--volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
	name_of_thing /bin/bash

### After running this, pause gazebo should pop up
roslaunch unitree_gazebo normal.launch rname:=a1 wname:=stairs_single 

## In a new termianl run (everthing below this is in this new terminal):

### Get the CONTAINER ID of the build
sudo docker ps

### Run the docker thing in this terminal
sudo docker exec -it $CONTAINER_ID$ bash

service ssh start

### The password is password
ssh root@localhost -p2233

cd /root/A1_ctrl_ws/

catkin build

source /root/A1_ctrl_ws/devel/setup.bash

echo "source /root/A1_ctrl_ws/devel/setup.bash" >> /.bashrc

### Run this command, and then pause the gazebo simulation
### This should reset the robot. Also press Control C so you can type another command after pausing
rosrun unitree_controller unitree_move_kinetic

### Run the mpc controller, and then unpause the gazebo sim to see the control work
roslaunch a1_cpp a1_ctrl.launch type:=gazebo solver_type:=mpc


