---
title: "Step-by-Step Guide to Installing and Using ROS Noetic on Ubuntu 20.04"
date: 2024-08-04 00:00:00 +0300
categories: [Study Notes, ROS Noetic]
tags: [ros-noetic, bash, python]     # TAG names should always be lowercase
math: true
image: assets/img/ros.png
---

In this post, we will look at how to install ROS Noetic on our system.

ROS Noetic, the latest LTS release supported until May 2025, is primarily targeted at Ubuntu 20.04 and doesn't officially support any newer distribution. The following installation steps are directly taken from the [ROS](https://wiki.ros.org/noetic/Installation/Ubuntu) website.

## 1. Installation

### 1.1. Setup Your sources.list

Set up your computer to accept software from packages.ros.org by adding the ROS repository to your `sources.list`:

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

This command adds the ROS package repository to your system's package sources, allowing you to install ROS packages.

### 1.2. Set Up Your Keys

Install `curl` if you haven't already, and then add the ROS key to your system:

```bash
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

This ensures that your system trusts the ROS package repository by adding the ROS GPG key.

### 1.3. Installation

First, update your package index:

```bash
sudo apt update
```

This updates your system's package list to include the newly added ROS repository.

If you want to work with Gazebo, it is highly recommended to install the full ROS Noetic desktop version, which includes Gazebo:

```bash
sudo apt install ros-noetic-desktop-full
```

### 1.4. Environment Setup

You must source the ROS setup script in every bash terminal you use ROS in. This sets up the environment variables necessary for ROS to function correctly:

```bash
source /opt/ros/noetic/setup.bash
```

To automatically source this script every time a new shell is launched, add it to your `~/.bashrc`:

```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 1.5. Dependencies for Building Packages

To create and manage your own ROS workspaces, you need additional tools and dependencies. Install them with:

```bash
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

#### 1.5.1 Initialize rosdep

Before using many ROS tools, you need to initialize rosdep. This tool enables you to easily install system dependencies for the source you want to compile. If you haven't installed rosdep, do so with:

```bash
sudo apt install python3-rosdep
```

Then, initialize rosdep with:

```bash
sudo rosdep init
rosdep update
```

Following these steps, you'll have a complete ROS Noetic installation on your Ubuntu 20.04 system, ready to start developing and running ROS packages.

## 2. Setting Up Environment and Some Basics

### 2.1 Checking the ROS Setup

Run the following command to check whether ROS is installed correctly:

```bash
roscore
```

`roscore` is the main control program of ROS. It must be running for any ROS nodes to communicate. You can close it with Ctrl+C if needed. Remember, roscore must be open in a terminal for your ROS projects to work.

### 2.2 Creating a Workspace

Create a new directory for your ROS workspace:

```bash
mkdir ~/test_ws/
cd test_ws/
mkdir src
catkin_make
```

In this setup:

- `test_ws` is the root directory of your workspace.
- `src` is the source directory where your packages will be stored.

The `catkin_make` command will create `devel` and `build` folders and create our workspace. When we create new packages, nodes, etc. we should use this command to make changes in our workspace.

### 2.3 Adding Environment Setup

We can type the command in our workspace directory `test_ws`:

```bash
source devel/setup.bash
```

every time when we open our terminal. The second option is permanent, adding the necessary setup script to your `~/.bashrc` to ensure that your environment is correctly set up every time you open a new terminal:

```bash
echo "source ~/test_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 2.4 Creating a Package

Create a new package within your workspace. A package is the main unit for organizing ROS code. The following command creates a package named `test_pkg` and specifies its dependencies (`std_msgs`, `rospy`, and `roscpp`):

```bash
cd ~/test_ws/src/
catkin_create_pkg test_pkg std_msgs rospy roscpp
```

This will create a folder named `test_pkg` in `src` folder.

### 2.5 Building the Workspace Again

Navigate to the root directory of your workspace and build the workspace:

```bash
cd ~/test_ws
catkin_make
```

The `catkin_make` command will build all the packages and changes in the workspace.


## 3. ROS Topics and Nodes

ROS communication is based on nodes and topics. Nodes are processes that perform computation, while topics are named channels over which nodes exchange messages.

### Publishers and Subscribers

- **Publisher**: A node that sends data to a topic. For instance, a sensor node might publish data about its readings to a topic called `/sensor_data`.

- **Subscriber**: A node that receives data from a topic. For example, a data processing node might subscribe to the `/sensor_data` topic to receive and process the sensor readings.

Nodes can both publish and subscribe to multiple topics, allowing for flexible communication patterns within a ROS network.

### 3.1 Creating and Running a Python Script

Navigate to the `src` directory of your package and create a Python script:

```bash
cd ~/robotics_ws/src/robotics/scripts/src
touch code.py 
sudo chmod +x code.py
```

The `touch` command creates a new file named `code.py`, and `chmod +x` makes the script executable. Now, let's write the Python code.

```python
#!/usr/bin/env python

import rospy
from std_msgs.msg import String

def publisher():

    pub = rospy.Publisher('robotics_first_topic', String, queue_size=10)
    rospy.init_node('publisher_node', anonymous=False)
    rate = rospy.Rate(10)  # 10 Hz
    while not rospy.is_shutdown():
        msg = "Our first publisher code!"
        rospy.loginfo(msg)
        pub.publish(msg)
        rate.sleep()

if __name__ == "__main__":
    publisher()
```

If you get an error because of the first line of the Python script, you can try the following command:

```bash
sudo apt install python-is-python3
```

We use the `rosrun` command to run our Python and C++ scripts. Now you can run `code.py` from any directory using the `rosrun` command:

```bash
rosrun scripts code.py
```

If you close `roscore`, you should first run `roscore` with the following command in a terminal again before executing the above commands.

```bash
roscore
```

Now, let's write the subscriber Python script `codesubscriber.py`.

```python
#!/usr/bin/env python

import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo("Our message is: %s", data.data)

def subs():
    # Initialize the node with a unique name
    rospy.init_node('listener', anonymous=False)

    # Subscribe to a topic with the specified name and message type
    rospy.Subscriber('robotics_first_topic', String, callback)

    # Keep the node running and processing callbacks
    rospy.spin()

if __name__ == "__main__":
    subs()
```

### How They Work Together

1. **Publisher Node**:
    - The publisher node continuously sends messages (`"Our first publisher code!"`) to the topic `robotics_first_topic`.

2. **Subscriber Node**:
    - The subscriber node listens to the same topic (`robotics_first_topic`).
    - When a message is published to this topic, the subscriber's callback function is triggered.
    - The callback function logs the received message.

### Example Execution

1. **Start roscore**:
    - Open a terminal and run `roscore` to start the ROS master.

2. **Run Publisher Node**:
    - Open a second terminal and run `rosrun scripts code.py` to start the publisher node.

3. **Run Subscriber Node**:
    - Open a third terminal and run `rosrun scripts codesubscriber.py` to start the subscriber node.

This command lists all the active topics.

```bash
rostopic list
```

We can listen the messages being published on the specified topic.

```bash
rostopic echo /robotics_first_topic
```

This command lists all active nodes.

```bash
rosnode list
```

And, this provides detailed information about a specified node.

```bash
rosnode info /listener
```
