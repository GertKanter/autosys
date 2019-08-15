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

## Some helpful code

```python
import tf

tf_listener = tf.TransformListener()
def get_robot_pose(name):
    try:
        translation, rotation = tf_listener.lookupTransform("map", name + "/base_footprint", rospy.Time(0))
    except (tf.LookupException, tf.ConnectivityException, tf.ExtrapolationException) as e:
        rospy.logwarn_throttle(3.0, e)
        rospy.sleep(0.1)
        return get_robot_pose(name)
    rospy.loginfo("Robot is at: %s" % translation)
    return (translation, rotation)
```

```python
import std_srvs.srv

def clear_costmaps(name):
    service_name = "/" + name + "/move_base_node/clear_costmaps"
    rospy.loginfo("Waiting for service %s" % service_name)
    rospy.wait_for_service(service_name)
    try:
	service = rospy.ServiceProxy(service_name, std_srvs.srv.Empty)
	rospy.loginfo("Calling service!")
	res = service()
	rospy.loginfo("Service finished!")
    except rospy.ServiceException, e:
	rospy.logerr("Service call failed: %s" % e)

```

```python
import actionlib
import move_base_msgs.msg

name = "robot_1"
move_base_client = actionlib.SimpleActionClient(name + '/move_base', move_base_msgs.msg.MoveBaseAction)

def goto_goal(name, goal, start, failure_counter=0):
    if failure_counter > 10:
	# Clear costmaps as last resort
	rospy.logerr("Trying to clear costmaps!")
	clear_costmaps(name)
	failure_counter = 0
    action_goal = move_base_msgs.msg.MoveBaseGoal()
    action_goal.target_pose.header.frame_id = "map"
    action_goal.target_pose.pose.position.x = goal['x']
    action_goal.target_pose.pose.position.y = goal['y']
    action_goal.target_pose.pose.position.z = 0
    action_goal.target_pose.pose.orientation.x = 0
    action_goal.target_pose.pose.orientation.y = 0
    action_goal.target_pose.pose.orientation.z = 0
    action_goal.target_pose.pose.orientation.w = 1
    move_base_client.wait_for_server()
    rospy.loginfo("Sending goal: %s" % action_goal)
    move_base_client.send_goal(action_goal)
    rospy.loginfo("Goal sent!")
    result = move_base_client.wait_for_result()

    state = move_base_client.get_state()
    if state == actionlib.GoalStatus.SUCCEEDED:
	# Make sure we succeeded by checking our pose vs goal
	update_pose()
	if euclidean_distance(self.pose[0], (goal['x'], goal['y'])) < 0.2:
	    return True
	else:
	    failure_counter += 1
	    rospy.sleep(1)
	    rospy.logerr("False SUCCEEDED information (failure=%s)!" % failure_counter)
	    goto_goal(start, goal, failure_counter)
	    rospy.sleep(1)
	    return goto_goal(goal, start, failure_counter) # Retry
    else:
	failure_counter += 1
	rospy.logwarn("Goal not SUCCEEDED, return to start (failure=%s)!" % failure_counter)
	rospy.sleep(1)
	goto_goal(start, goal, failure_counter)
	rospy.sleep(1)
	return goto_goal(goal, start, failure_counter)

```
