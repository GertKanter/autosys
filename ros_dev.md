# Development in ROS

You can get a copy of a free book [here](http://dijkstra.cs.ttu.ee/~Gert.Kanter/autosys/ROS_Robot_Programming_EN.pdf).

[Developer guide](http://wiki.ros.org/DevelopersGuide) for ROS1.

A list of [best practices](https://github.com/leggedrobotics/ros_best_practices/wiki) from ETH Zurich Legged Robotics group.

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