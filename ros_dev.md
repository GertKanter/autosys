# Development in ROS

Dev for ROS [lecture slides](http://courses.csail.mit.edu/6.141/spring2012/pub/lectures/Lec06-ROS.pdf) from MIT.

ROS tutorials [link](http://wiki.ros.org/ROS/Tutorials)

Beginner tutorials for developing in Python are [here](http://wiki.ros.org/rospy_tutorials/Tutorials).

There is a free book called "ROS Robot Programming" written by the TB3 developers (published by ROBOTIS). You can get a copy of the book [here](https://community.robotsource.org/t/download-the-ros-robot-programming-book-for-free/51).

[Developer guide](http://wiki.ros.org/DevelopersGuide) for ROS1.

A list of [best practices](https://github.com/leggedrobotics/ros_best_practices/wiki) from ETH Zurich Legged Robotics group.

## Note about ROS2

We're not focusing on ROS2 in this school but a starting point for this journey can be [this](https://github.com/ros2/ros2/wiki/Colcon-Tutorial) tutorial. As there are very few guides/materials about ROS2 and ROS2 is still under heavy development it is not recommended for inexperienced developers (this is especially true for testing).

# Unit testing in ROS

[Unit testing](http://wiki.ros.org/action/show/Quality/Tutorials/UnitTesting?action=show&redirect=UnitTesting) tutorial.

Good resource for learning "by example" is the [rospy_tutorials](https://github.com/ros/ros_tutorials/tree/melodic-devel/rospy_tutorials)

## rostest

`rostest` is very similar to `roslaunch`. The main difference is that it is designed to launch the tests as well as launching the nodes.

Good tutorials available at [ros wiki](http://wiki.ros.org/rostest).

## Workflow
In general, for both Python unit and integration tests you want to do the following:

* Create a class that subclasses from `unittest.TestCase` or other testing framework (e.g., pytest (see this [package](https://github.com/machinekoder/ros_pytest)))
* Have a bunch of methods with the prefix "test_". Each represents a test case
* Implement your tests in these methods using the assert*() calls (e.g.self.assertTrue)
* Integration tests are the same except you create a ROS node handle within the test case and start publishing/subscribing. You perform checks in the exact same way as you do with unittest
* For integration tests, you also want to add a `add_rostest` to your CMakeLists.txt file.
* `catkin_make run_tests` in your catkin workspace

## gtest

`gtest` is used for testing C++ nodes. There is documentation available at the [ros wiki](http://wiki.ros.org/gtest)

## ROS2 notes

There are [guidelines](https://github.com/ros2/ros2/wiki/Developer-Guide#testing) for testing and quality assurance requirements for ROS2 packages.

Requirements to be considered a 'Level 1' package:

* Have a strictly declared public API
* Have API documentation coverage for public symbols
* Have 100 percent branch code coverage from unit and integration tests
* Have system tests which cover any scenarios covered in documentation
* Have system tests for any corner cases encountered during testing
* Must be >= version 1.0.0

# Testing using rosbag files

To test your node with previously recorded data you can use `rosbag` files. A good example of this is the [AMCL package](https://github.com/ros-planning/navigation/tree/melodic-devel/amcl)
