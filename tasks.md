# Tasks

## Create workspace

[Tutorial](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment)

We already have a `~/catkin_ws` workspace, try creating a new workspace with another name (directory name).

## Create a ROS package

[Tutorial](http://wiki.ros.org/ROS/Tutorials/CreatingPackage)

Create a package in the new workspace.

## Write a simple publisher/subscriber in Python

[Tutorial link](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29)

Create a node in the package that can publish and subscribe to topics.

## Write a rostest

[Tutorial link](rostest.md)

Create some unit tests for your node.

# Optional tasks

## Create a ROS msg and srv

[Tutorial](http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv)

Creating new message types and services is useful and in many cases good practice but not strictly necessary for this course.

## Recording and playing back data

[Tutorial link](http://wiki.ros.org/ROS/Tutorials/Recording%20and%20playing%20back%20data)

Recording data from ROS is very useful as it allows debugging and analysis of the behaviour after the fact.

Playing back data can also be used for testing as the played back data seems to be "real" for the subscribed nodes.

## Using roslaunch and rqt console

[Tutorial link](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch)

Launching many nodes together is very common in ROS (starting up the robot) and it is achieved with `roslaunch`.

In real-world scenarios, there are multiple nodes producing some (debugging) output and you want to filter and analyze them one at a time (`rqt_console` helps).
