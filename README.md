# Lab example

![](https://github.com/GertKanter/autosys/tree/master/images/screenshot.png "This is what is looks like")

## Running the stack
Start the Autosys VMWare virtual machine.

Login username is `user` and password `user`.

Once you have logged in, start opening terminals (shortcut Ctrl+Alt+T).

### Terminal 1 (ROS Master + Stage)
Then you can start a ROS master and load the Stage simulator with one robot by executing
```
docker run -it --rm --network=host stage bash -c "source /catkin_ws/devel/setup.bash && roslaunch robots_stage stage.launch"
```

### Terminal 2 (RViz, optional)
To visualize what is happening with the robot, run this command in a new terminal
```
rosrun rviz rviz
```

### Terminal 3 (Spread daemon for UPPAAL-DTRON-ROS adapter)
In the next  terminal run the DTRON ROS adapter container with Spread daemon
```
docker run -it --rm --network=host -v ~/models:/catkin_ws/src/testit_dtron/testit_dtron/dtron/models --name testit_dtron testit_dtron:tron_dev /catkin_ws/spread/sbin/spread -n localhost
```

### Terminal 4 (UPPAAL-DTRON-ROS adapter)
In another terminal run the DTRON ROS adapter
```
docker exec -it testit_dtron bash -c "source /catkin_ws/devel/setup.bash && roslaunch testit_dtron run_with_load_params.launch"
```

### Terminal 5 (DTRON)
In this terminal run DTRON with the example UPPAAL model
```
docker exec -it  testit_dtron bash -c 'source /catkin_ws/devel/setup.bash && rosrun testit_dtron run_dtron_test.sh $(rospack find testit_dtron)/dtron/example.xml'
```

---

If you want to run DTRON test with your own model, place the model in the `~/models` directory and use this command (change the filename if you need to):
```
docker exec -it  testit_dtron bash -c 'source /catkin_ws/devel/setup.bash && rosrun testit_dtron run_dtron_test.sh $(rospack find testit_dtron)/dtron/models/model.xml'
```

## Shutting down the stack
To shut down the stack, just press Ctrl+C in each terminal and that's it (the docker containers are ephemeral and are removed after shutting down).