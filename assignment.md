# Course assignment

In short: implement an algorithm for a guard robot to detect an intruder.

It is useful to divide the whole task into parts to simplify solving it.

This particular problem we can divide into two distinct parts: planning and detection.

For example, we can create two packages `autosys_planner` and `autosys_detector`.

## Planner

The planner has to implement the same algorithm that you designed in UPPAAL.

### Configuration

* Name/ID (e.g., `robot_1`)
* Map ([map.yaml](map.yaml))

### Input

* Information published by other robots (i.e., other robots' poses)
* Signals from detector node (robot detected)

### Output

* Goals where to move (move_base_msgs.msg.MoveBaseGoal())
* Information to other robots where the robot currently is (publish to `/broadcast` topic, type `geometry_msgs.msg.PoseStamped`)

## Detector

Each robot is autonomous and decentralized without a supervisor. This means that each robot does not know how many other guard robots there are and where they are "automatically" - this information needs to be communicated by the robots themselves.

### Configuration

* name/ID (e.g., `robot_1`)

### Input

* Laser sensor information (sensor_msgs.msg.LaserScan, topic is `"/" + name + "/base_scan"`)

### Output

* Signal that a robot has been detected (based on laser sensor information), type: `geometry_msgs.msg.PointStamped`

Hint: X-axis of the laser points forward (frame)
