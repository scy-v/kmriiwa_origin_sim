## Contents:

1. Running Moveit fake controller  
2. Navigation + Moveit + Gazebo + simulation environment running  
3. Moveit standalone control for real robot arm  
4. Moveit combined control for real robot arm + base planning  
5. Moveit standalone control for real onRobot gripper  
6. Moveit combined control for real robot arm + onRobot gripper  
7. Moveit combined control for real robot arm + onRobot gripper + base  
8. Running Gazebo simulation demo  
9. Running real robot demo  
10. Explanation of parameters  
11. Problem description  

## Running fake controller  
```shell
roslaunch kmriiwa_moveit move_group.launch fake_execution:=true load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=false load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=false load_real_move_base:=false
```

- In rviz’s Motion Planning, select the kmriiwa_manipulator Planning Group to do fake planning for the robot arm, or select the gripper Planning Group to do fake planning for the onRobot gripper. The rest runs similarly.
  
## Navigation + Moveit + Gazebo simulation environment running
```shell
roslaunch kmriiwa_gazebo gazebo_pick_place_demo.launch  load_realsense:=false 
# start gazebo
roslaunch kmriiwa_bringup planning_stack_bringup.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=true load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=false load_move_group_rviz:=false load_real_move_base:=false
# start move_group/navigation/rviz

# If you don’t want to start navigation and only start move_group/rviz, then:
roslaunch kmriiwa_moveit move_group.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=true load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=false load_real_move_base:=false load_move_group_rviz:=true
```
- In rviz, open the top Panels menu, click Tools, then click 2D Nav Goal for navigation planning, and 2D Pose Estimate to localize the robot on the map.
- 
## Moveit standalone control for real robot arm
```shell
roslaunch kmriiwa_moveit move_group.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=false load_real_RG2FT:=false load_real_kmriiwa:=true load_realsense:=true load_real_move_base:=false
```

## Moveit combined control for real robot arm + base
```shell
roslaunch kmriiwa_bringup planning_stack_bringup.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=false load_real_RG2FT:=false load_real_kmriiwa:=true load_realsense:=true load_real_move_base:=true
```

## Moveit standalone control for real onRobot gripper
```shell
roslaunch kmriiwa_moveit move_group.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=false load_real_RG2FT:=true load_real_kmriiwa:=false load_realsense:=true load_real_move_base:=false

```
## Moveit combined control for real robot arm + onRobot gripper
```shell
roslaunch kmriiwa_moveit move_group.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=true load_simulation:=false load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=true load_real_move_base:=false
```
## Moveit combined control for real robot arm + onRobot gripper + base
```shell
roslaunch kmriiwa_bringup planning_stack_bringup.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=true load_simulation:=false load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=true load_real_move_base:=true
```
## Running simulation demo
```shell
roslaunch kmriiwa_gazebo gazebo_pick_place_demo.launch  load_realsense:=false
# start gazebo
roslaunch kmriiwa_bringup planning_stack_bringup.launch load_realsense_topic:=false merge_real_RG2FT_kmriiwa:=false load_simulation:=true load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=false load_move_group_rviz:=false load_real_move_base:=false
# start move_group/navigation/rviz
roslaunch kmriiwa_simulation_grasp find_object.launch load_realsense:=false load_real_world:=false
# object detection, get object pose
rosrun deep_grasp_task tf_transform_cylinder.py
# publish object pose tf
roslaunch kmriiwa_simulation_grasp demo.launch  load_realsense:=false load_real_world:=false
# start demo program
```
## Running real robot demo
```shell
roslaunch kmriiwa_bringup planning_stack_bringup.launch load_realsense_topic:=true merge_real_RG2FT_kmriiwa:=true load_simulation:=false load_real_RG2FT:=false load_real_kmriiwa:=false load_realsense:=true load_move_group_rviz:=false load_real_move_base:=true
# start move_group/navigation/rviz
roslaunch kmriiwa_simulation_grasp find_object.launch load_realsense:=true load_real_world:=true
# object detection, get object pose
rosrun deep_grasp_task tf_transform_cylinder.py
# publish object pose tf
roslaunch kmriiwa_simulation_grasp demo.launch  load_realsense:=false load_real_world:=true
# start demo program
```

## Explanation of parameters

1. To change navigation speed, check out:
```shell
kmriiwa_ros_stack/kmriiwa_navigation/config/local_planner.yaml
```

2. To change obstacle radius for local and global maps, check out:
```shell
src/kmriiwa_ros_stack/kmriiwa_navigation/config/local_costmap.yaml
src/kmriiwa_ros_stack/kmriiwa_navigation/config/global_costmap_static_map.yaml
```

3. If the camera position on the robot changes, these files are used to change the transform from camera coordinates to base coordinates when running simulation or real robot demo:
```shell
src/deep_grasp_demo/deep_grasp_task/config/calib/camera_real.yaml
src/deep_grasp_demo/deep_grasp_task/config/calib/camera_sim.yaml
```

4. These files are used to adjust parameters when running simulation or real robot demo:
```shell
src/deep_grasp_demo/deep_grasp_task/config/kmriiwa_object_real.yaml
src/deep_grasp_demo/deep_grasp_task/config/kmriiwa_object_sim.yaml
```
Main parameters to tune include:
```shell
1. Scene frames: used to set the reference coordinate system  
2. move_base_target: sets the navigation position relative to the reference frame  
3. Spawn_cylinder and Spawn_table set to true means these two objects need to be loaded, adjust their shape and pose  
4. When running the demo, the cylinder pose setting in the file has no effect because its pose is set by the object detection results subscribed during runtime  
5. grasp_frame_transform: determines the final pose of the robot arm before grabbing the object, i.e. the hand_frame relative to the cylinder_frame  
6. cylinder_place_pose: determines the position for placing the cylinder relative to the reference frame  
7. approach_object_min/max_dist: the pose in point 5 is not reached directly but first moves to the same orientation, then moves by this distance to the final pose  
8. lift_object_min/max_dist: controls the height distance when lifting the object  
```
## Problem description

1. In simulation, both realsense and kinect cameras can be used, but the realsense point cloud is generated by cropping depth images and takes more resources than kinect, causing lag. Also, the simulated realsense camera is connected based on the real camera pose. If its pose is changed, it won’t directly work in simulation. To use and run the demo, you need to adjust the camera pose and related demo parameters. It is recommended to use kinect by setting load_realsense:=false.

2. The real robot uses a realsense camera, but in demo.launch load_realsense:=false is set because this parameter is used to distinguish topic names. load_realsense:=true loads the realsense simulated point cloud topic; false loads the kinect simulated point cloud topic. The real realsense topic name is set to be the same as the simulated kinect, so it directly loads the simulated kinect topic.
