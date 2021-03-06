# How to launch original env
 First launch the simulator
  
  ``` 
  roslaunch ur_robotiq_gazebo gym.launch
  ```
 
 And run the training launch
  ```
  roslaunch ur_training default.launch
  ```

## Conveyer GAZEBO env

First launch the gazebo and gym interface and node publishing block point.
 ```
 roslaunch ur_robotiq_gazebo conveyer_gym.launch --screen
 ```
 
 Run the RL algorithms and unpause the GAZEBO
  ```
  roslaunch ur_training default.launch
  ```
 

> Latest block's point:
``` 
rostopic echo /target_blocks_pose
```

> Total block's points:
``` 
rostopic echo /blocks_poses 
```


# How to launch REINFORCE algorithm
 First launch the simulator
  
  ``` 
roslaunch ur_robotiq_gazebo conveyer_gym.launch controller:=vel --screen gui:=false
  ```
 
 And load the parameters and launch python file for reset
  ```
roslaunch ur_reaching reinforcement.launch
  ```

 And start the learning algorithm 
  ```
python reinforcement_main.py 
  ```

 And unpause physics of GAZEBO simulator
 ```
 rosservice call /gazebo/unpause_physics "{}"
 ```



# How to launch PPO+GAE algorithm

First launch the simulator including loading the parameters and GAZEBO Excution func
```
roslaunch ur_robotiq_gazebo conveyer_gym.launch --screen gui:=false
```

 And start the learning algorithm 
 ```
  python ppo_gae_main.py
 ```


# How to use the RLkit 
[RLkit](https://github.com/vitchyr/rlkit) is reinforcement learning framework based on [rllab](https://github.com/rll/rllab)

## Run GAZEBO simulator and gazebo_execution 
First launch the simulator including loading the parameters and GAZEBO Excution func
```
roslaunch ur_robotiq_gazebo conveyer_gym.launch --screen gui:=false
```

## Training
 And start the SAC learning algorithm based on RLkit
 ```
  python rlkit_sac_main.py
 ```

 And unpause physics of GAZEBO simulator
 ```
 rosservice call /gazebo/unpause_physics "{}"
 ```
 
 After training, you may find the pickled files on the rlkit/data folder.
 
 you can easily see the results through selecting the generated folder about training like follwing:
 
 ```
 python viskit/frontend.py ../rlkit/data/SAC/SAC_2019_10_14_08_27_55_0000--s-0/
```

## Evaluation

If you want to evaluate the trained weight, you can run like following:
```
python rlkit/scripts/run_policy.py rlkit/data/SAC/SAC_2019_10_14_08_27_55_0000--s-0/params.pkl 
```


## Visualization

- Rviz
```
roslaunch ur_robotiq_moveit_config  moveit_rviz.launch 
```

- rviz_visual_tools
```
rosrun visual_tools rviz_visual_tools_demo
```

![Visualization](../images/rviz_visual_tools.png)

- moveit_visual_tools
```
rosrun visual_tools moveit_visual_tools_demo
```




## Services

- _/gazebo/unpause_physics_ ([Empty](http://docs.ros.org/melodic/api/std_srvs/html/srv/Empty.html))
  - Unpause GAZEBO simulator for starting
  ```
  rosservice call /gazebo/unpause_physics "{}"
  ```
- _/set_velocity_controller_ ([SetBool](http://docs.ros.org/melodic/api/std_srvs/html/srv/SetBool.html))
  -  Set velocity controllers including 6 velocity controllers after stoppint velocity_controller/JointTrajectoryController
- _/set_trajectory_velocity_controller_ ([SetBool](http://docs.ros.org/melodic/api/std_srvs/html/srv/SetBool.html))
  -  Set velocity trajectory controller including Joint trajectory controllers after stopping velocity_controller/JointVelocityController
- _/stop_training_ ([SetBool](http://docs.ros.org/melodic/api/std_srvs/html/srv/SetBool.html))
  -  Stop training process
- _/start_training_ ([SetBool](http://docs.ros.org/melodic/api/std_srvs/html/srv/SetBool.html))
  -  Start training process


## Helpful function
We can check the controller manager status using following:
```
rosrun rqt_controller_manager rqt_controller_manager
```


![controller manager](../images/controller_manager.png)
