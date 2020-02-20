---
title: 'Aerial Manipulation'
subtitle: 'This project aims to build an Aerial Manipulation system (a robotic arm under a drone).'
date: 2018-06-30 00:00:00
featured_image: '/images/projects/AM.jpg'
---

## Introduction 

What are the applications of drones nowadays? Aerial photography, package delivery and detection in unknown environment. How about manipulators? The most common application is assembly tasks in factory. We admit that drones and manipulators benefit a lot in our life, but they still have some weaknesses. For the drones, we do see one common weakness of payload capacity in drones, and drones with specific payload are only worked for just one specific task. This weakness greatly limits the application of drones. There is also weaknesses in manipulators. The base of each manipulator is fixed on the ground and this design greatly restricts the workspace of the end-effector and hence limits the applications of manipulators. 

Therefore, what if we mount the manipulator on the drone to make the robotic arm move freely in the whole 3D space? Under this assumption, the mobility of the drone will greatly enlarge the effective workspace of the robotic arm, and a dexterous robotic arm with a moving base will have countless possible applications in the future as well, something that a drone or a fixed manipulator cannot do solely. And this is the motivation of my research to build a what we call aerial manipulation system and contribute to our life.

![](/images/projects/AM.jpg)

Imagine this — when you go to the supermarket, you could select your groceries through a tablet placed at the front gate and the aerial manipulators then would bring the groceries you need into your cart. Or after making an order with an iPad in a restaurant, your “aerial waiter” would serve your order, refill your drink and bring check to you when you finish. We do not even have to mention that aerial manipulator shall play an important role in many specific professional fields and replace humans for dangerous works. 

However, you can imagine how hard it is to make this system hover steadily when the arm under it is moving or operating something. We need to admit that although the drone greatly enlarges the effective workspace of end-effector, it also makes the base of robot arm to be unstable or unreliable for manipulation tasks. 

One way to solve the stabilization problem is to just tune the existing controller of the drone and regard the manipulator as disturbances. This method assumes that the weight of robotic arm is greatly less than the drone, I tested that recently and the performance is not quite well when we attached some weight on the arm. 

I am now trying to remodel the whole rigid body and analyze the dynamic properties of the system and rewrite the control algorithms to try to stabilize it  as much as possible.

My ultimate goal of this research is not just build the system and control it but to make the aerial manipulation system cooperate with human to do some daily tasks. Therefore, making the AM move as human desired is the unavoidable first step. I hope that this flying arm could benefit our life in the near future. Thank you.

## Video

The following is a video of the Aerial Manipulation system test flying with the arm moving.

<iframe href="https://www.youtube.com/watch?v=kvRFQ7bfAXk" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

## Simulation

This simulation is based on the following git repo:
    https://github.com/AtsushiSakai/PythonRobotics
    
All the code for simulation is in drone_3d_trajectory_following folder. 
    
    python drone_simulation.py

Here is the gif of the simulation

This simulation is for drone hovering with disturbances. Those disturbances is to check whether the controller is work.

## Hardware implementation

We use Navio2 as our main control board. The Navio2 is attached on the Respberry pi B+ and give PWM to the ESC of the drone.

The navio folder contains some neccesary file for hardware read and out.

The indoor localization is realized by using 6 cameras optitrack motion capture system. We use ROS package vrpn_client_ros to receive the streaming data from ground station of optitrack.

The ROS topic is converted to basic numpy array and sending to the control board of the drone by using zeromq technique.

The diagram for the system is showing in the following figure:

![](http://github.com/Zhi29/droneproject/raw/master/pic/system.png)

#### github repository
[The codes for this project](https://github.com/Zhi29/droneproject)
