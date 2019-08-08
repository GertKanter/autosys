# Autonomous Systems School

## Development in ROS

Software development using ROS [lecture slides](http://courses.csail.mit.edu/6.141/spring2012/pub/lectures/Lec06-ROS.pdf) from MIT.

[ROS Industrial (Melodic) training exercises](https://industrial-training-master.readthedocs.io/en/melodic/)

[ROS tutorials](http://wiki.ros.org/ROS/Tutorials)

Beginner tutorials for developing in Python are [here](http://wiki.ros.org/rospy_tutorials/Tutorials).

There is a free book called "ROS Robot Programming" written by the TB3 developers (published by ROBOTIS). You can get a copy of the book [here](https://community.robotsource.org/t/download-the-ros-robot-programming-book-for-free/51).

[Developer guide](http://wiki.ros.org/DevelopersGuide) for ROS1.

A list of [best practices](https://github.com/leggedrobotics/ros_best_practices/wiki) from ETH Zurich Legged Robotics group.

### Note about ROS2

Why not use ROS2 in this course?
ROS2 Dashing Diademata was released at the end of May 2019. As it currently stands, there are still many packages that have not been ported to ROS2 and there are still significant changes made to the design and build systems. There is also a lack of good cohesive documentation/tutorials and examples which increases the learning difficulty. As this is not strictly a ROS course, we believe that it will be easier and more time efficient to understand the big picture using ROS1.

An article about the reasoning [why ROS2](http://design.ros2.org/articles/why_ros2.html) is needed.

## Unit testing in ROS

[Unit testing](http://wiki.ros.org/action/show/Quality/Tutorials/UnitTesting?action=show&redirect=UnitTesting) tutorial.

Good resource for learning "by example" is the [rospy_tutorials](https://github.com/ros/ros_tutorials/tree/melodic-devel/rospy_tutorials)

### rostest

`rostest` is very similar to `roslaunch`. The main difference is that it is designed to launch the tests as well as launching the nodes.

Good tutorials available at [ros wiki](http://wiki.ros.org/rostest).

### Workflow
In general, for both Python unit and integration tests you want to do the following:

* Create a class that subclasses from `unittest.TestCase` or other testing framework (e.g., pytest (see this [package](https://github.com/machinekoder/ros_pytest)))
* Have a bunch of methods with the prefix "test_". Each represents a test case
* Implement your tests in these methods using the assert*() calls (e.g.self.assertTrue)
* Integration tests are the same except you create a ROS node handle within the test case and start publishing/subscribing. You perform checks in the exact same way as you do with unittest
* For integration tests, you also want to add a `add_rostest` to your CMakeLists.txt file.
* `catkin_make run_tests` in your catkin workspace

### gtest

`gtest` is used for testing C++ nodes. There is documentation available at the [ros wiki](http://wiki.ros.org/gtest)

### ROS2 notes

There are [guidelines](https://index.ros.org/doc/ros2/Contributing/Developer-Guide/) for testing and quality assurance requirements for ROS2 packages.

## Testing using rosbag files

To test your node with previously recorded data you can use `rosbag` files. A good example of this is the [AMCL package](https://github.com/ros-planning/navigation/tree/melodic-devel/amcl)
