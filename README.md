# Lab example
Start the Autosys VMWare virtual machine.
Then you can start a ROS master and load the Stage simulator with one robot by executing
```
docker run -it --rm --network=host stage bash -c "source /catkin_ws/devel/setup.bash && roslaunch robots_stage stage.launch"
```

To visualize what is happening with the robot, run this command in a new terminal
```
rosrun rviz rviz
```

In the second terminal run the DTRON ROS adapter container with Spread daemon
```
docker run -it --rm --network=host -v ~/models:/catkin_ws/src/testit_dtron/testit_dtron/dtron/models --name testit_dtron testit_dtron:tron_dev /catkin_ws/spread/sbin/spread -n localhost
```

In another terminal run the DTRON ROS adapter
```
docker exec -it testit_dtron bash -c "source /catkin_ws/devel/setup.bash && roslaunch testit_dtron run_with_load_params.launch"
```

In the third terminal run DTRON with the example UPPAAL model
```
docker exec -it  testit_dtron bash -c 'source /catkin_ws/devel/setup.bash && rosrun testit_dtron run_dtron_test.sh $(rospack find testit_dtron)/dtron/example.xml'
```

---

If you want to run DTRON test with your own model, place the model in the `~/models` directory and use this command (change the filename if you need to):
```
docker exec -it  testit_dtron bash -c 'source /catkin_ws/devel/setup.bash && rosrun testit_dtron run_dtron_test.sh $(rospack find testit_dtron)/dtron/models/model.xml'
```
