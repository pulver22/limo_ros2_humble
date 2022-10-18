# Running on the Jetson Nano:
```shell
# Create local workspace
mkdir -p ~/agx_workspace
cd ~/agx_workspace
# Clone the project
git clone https://github.com/agilexrobotics/limo_ros2.git src

mv ~/agx_workspace/src/.devcontainer ~/agx_workspace
```
`` VS Code remote limo, ~/agx_workspace reopen in container``
 ``[Recommend] Login the limo via VS Code remote plugin, open ~/agx_workspace.Then select reopen in container in the menu``



``Or running automatically setup script``
```shell
cd ~/agx_workspace/src
chmod +x setup.sh
./docker_setup.sh
```
Then follow the prompts

# Simulation:
After installing ROS2 Eloquent [see documents here](docs/README.md) and setting up the software and environment run the following scripts one by one, being careful not to run in the background

# Preparing
```shell
# Create local workspace
mkdir -p ~/agx_workspace
cd ~/agx_workspace
# Clone the project
git clone https://github.com/agilexrobotics/limo_ros2.git src

# Install essential packages
apt-get update \
    && apt-get install -y --no-install-recommends \	
    libusb-1.0-0 \
    udev \
    apt-transport-https \
    ca-certificates \
    curl \
    swig \
    software-properties-common \
    python3-pip


# Install ydlidar driver
git clone https://ghproxy.com/https://github.com/YDLIDAR/YDLidar-SDK.git &&\
    mkdir -p YDLidar-SDK/build && \
    cd YDLidar-SDK/build &&\
    cmake ..&&\
    make &&\
    make install &&\
    cd .. &&\
    pip install . &&\
    cd .. && rm -r YDLidar-SDK 

# Compile limo_ros2 packages
cd ~/agx_workspace
catkin_make
source devel/setup.bash
```

# Navigation

```shell
rviz2
## start the chassis
ros2 launch limo_bringup limo_start.launch.py
sleep 2

## start navigation
ros2 launch limo_bringup limo_navigation.launch.py
sleep 2

## start positioning
ros2 launch limo_bringup limo_localization.launch.py
```

# start positioning

```shell
rviz2
ros2 launch limo_bringup limo_start.launch.py
ros2 launch build_map_2d revo_build_map_2d.launch.py
# After the above three command are activated use a separate screen to control the car
```


 keyboard control

```shell
ros2 launch limo_bringup limo_start.launch.py
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

# Simulation

##  Four wheel differential mode

Display the model in rviz2

```
ros2 launch limo_description display_models_diff.launch.py 
```

Run the simulation in gazebo

```
ros2 launch limo_description gazebo_models_diff.launch.py 
```

Start keyboard teleop to control Limo

```
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

## Ackermann Model

In development





