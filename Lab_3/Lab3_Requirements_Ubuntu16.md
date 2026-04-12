# LAB 3 REQUIREMENTS INSTALL GUIDE — UBUNTU 16.04

Important:
This guide installs everything needed for Lab 3 on Ubuntu 16.04:
- ROS Kinetic
- RViz
- useful ROS tools
- Git
- the epuck_vrep_ros lab files
- a simulator path for the e-puck model

---

## PART 1 - INSTALL ROS KINETIC

1) Add the ROS repo

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu xenial main" > /etc/apt/sources.list.d/ros-latest.list'
```

2) Add the ROS key

```bash
sudo apt install -y curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

3) Update package lists

```bash
sudo apt update
```

4) Install full ROS desktop

```bash
sudo apt install -y ros-kinetic-desktop-full
```

5) Initialize rosdep

```bash
sudo rosdep init
rosdep update
```

6) Make ROS load automatically in every terminal

```bash
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

7) Install useful ROS tools

```bash
sudo apt install -y python-rosinstall python-rosinstall-generator python-wstool build-essential
```

8) Install git

```bash
sudo apt install -y git
```

---

## PART 2 - TEST ROS

Open TERMINAL 1:

```bash
roscore
```

Leave it open.

Open TERMINAL 2:

```bash
source /opt/ros/kinetic/setup.bash
rostopic list
```

You should see at least:
- `/rosout`
- `/rosout_agg`

Open TERMINAL 3:

```bash
source /opt/ros/kinetic/setup.bash
rosrun rviz rviz
```

If RViz opens, ROS + RViz are working.

---

## PART 3 - DOWNLOAD THE LAB FILES

Open a new terminal and run:

```bash
cd ~
git clone https://github.com/andrei91ro/epuck_vrep_ros.git
ls ~/epuck_vrep_ros
```

Check that these files exist:

```bash
ls ~/epuck_vrep_ros/e-puck.ttm
ls ~/epuck_vrep_ros/vrep_single_epuck.rviz
ls ~/epuck_vrep_ros/epuck_vrep_ros.lua
```

---

## PART 4 - INSTALL THE SIMULATOR

Download CoppeliaSim Edu for Ubuntu 16.04 from Coppelia previous versions.
Save it in `~/Downloads`, then run:

```bash
cd ~/Downloads
tar -xJf CoppeliaSim*.tar.xz
sudo mv CoppeliaSim* /opt/coppeliasim
sudo ln -sf /opt/coppeliasim/coppeliaSim.sh /usr/local/bin/vrep
sudo chmod +x /opt/coppeliasim/coppeliaSim.sh
```

Check:

```bash
which vrep
vrep
```

---

## PART 5 - IF THE SIMULATOR FAILS TO START

Install common graphics/runtime libraries:

```bash
sudo apt update
sudo apt install -y \
  libglib2.0-0 \
  libgl1-mesa-glx \
  libxi6 \
  libdbus-1-3 \
  libfontconfig1 \
  libxrender1 \
  libxrandr2 \
  libxinerama1 \
  libxcursor1 \
  libx11-6 \
  libxcb1 \
  libglu1-mesa
```

Then test again:

```bash
vrep
```

---

## PART 6 - FINAL CHECK FOR LAB 3

Check ROS:

```bash
echo $ROS_DISTRO
which roscore
which rosrun
which rviz
```

Check files:

```bash
ls ~/epuck_vrep_ros/e-puck.ttm
ls ~/epuck_vrep_ros/vrep_single_epuck.rviz
```

Check simulator:

```bash
which vrep
```

---

## PART 7 - FIRST LAB 3 LAUNCH

TERMINAL 1:

```bash
source /opt/ros/kinetic/setup.bash
roscore
```

TERMINAL 2:

```bash
source /opt/ros/kinetic/setup.bash
vrep ~/epuck_vrep_ros/e-puck.ttm
```

Before starting simulation:
set the e-puck parameter:
`opMode = 2`

TERMINAL 3:

```bash
source /opt/ros/kinetic/setup.bash
rosrun rviz rviz -d ~/epuck_vrep_ros/vrep_single_epuck.rviz
```

TERMINAL 4:

```bash
source /opt/ros/kinetic/setup.bash
rostopic list
```
