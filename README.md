# Autonomous Systems School

## Lab setup and tips

See the documentation [here](lab_stack.md).

See [lab tips](lab_tips.md) for tips.

## Task

### ROS Development

Get started [here](ros_dev.md).

### Overview

The task is to create a ROS package to give an alert whenever an object is too close to the robot. The input to the algorithm is the sensor data from LIDAR (type `LaserScan`).

As part of the task, you have to create unit tests for the algorithm.

### Technical details

The package should be named `proximity_alert`.

The package input (by default) should be from the robot namespace topic named `base_scan`. You need to pay attention to namespaces as the global topic name is `/robot_0/base_scan` (it could `robot_0`, `robot_1`, `or_whatever_else`).

The input message type is `sensor_msgs/LaserScan`. [Link to definition](http://docs.ros.org/melodic/api/sensor_msgs/html/msg/LaserScan.html).

The output of the algorithm should be published as a Boolean message (`std_msgs/Bool`). [Link to definition](http://docs.ros.org/api/std_msgs/html/msg/Bool.html). The result should be `false` if there are no obstacles detected and `true` if there is an obstacle closer than the specified distance.

The alert distance should be customizable (e.g., passed as a variable on start-up) and by default should be 2.0 meters.

The output topic should be customizable but default is `proximity_alert` in the robot namespace (e.g., `/robot_0/proximity_alert`).

The output should be published 10 times per second (10 Hz rate).

### Task levels
#### Basic level
The package subscribes to the LIDAR data as specified and outputs the result as specified in the previous section.

#### Task extension options
Once the basic node is finished, you can extend it as time permits. Some ideas are listed as follows
- Only give an alert is the obstacle is almost directly ahead of the robot (angle +/- 0.05 rad, customizable)
- Give an alert whenever you are close to an obstacle that you have observed at any time even if you cannot see the obstacle currently (i.e., the obstacle is behind the robot). To achieve this, you should use the TF library (See [documentation here](http://wiki.ros.org/tf)) and store the objects in memory.
- Analyze the trajectory of the robot after it has started moving to a goal and stop the robot if you detect that the robot's trajectory will come too close to an obstacle before it actually reaches it (preemptive behavior).
- Allow specification of the obstacle signature (what kinds of objects are considered obstacles for the proximity alert and which should be ignored). For example, narrow cylinders, cubes, two cylinders side-by-side etc. Also publish the type of the obstacle detected.
- Add support for multi-robot shared obstacle information. Define a topic and publish the detected objects to this channel and also add new objects that other robots detect.
