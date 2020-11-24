.. _EEHal:

.. role:: raw-html(raw)
  :format: html
  
ROS End-Effector HAL
=========================================

ROS End-Effector relies on a HAL interface (read: C++ pure abstract class) to communicate with simulated or real robot. This permits to load at run-time a concrete HAL depending on the robot that we are controlling.
In general, the HAL will take actuator position commands as ros messages (from ROS End-Effector) and will give back the actual position of the robot's actuators, again as ros messages. The communication with the robot (i.e. sense() and move()) must be implemented by each concrete HAL

Included concrete Hal
#######################

We already provide two Hals. These can be check as examples to understand how to implement a new custom HAL.

DummyHal
*********

This Hal simply propagate the commands to other ROS topics, read by rviz and/or Gazebo. It is suitable for easy and fast ROS End-Effector use with simulated robots

XBot2Hal
**********

As above, this HAL communicate with simulated robot, but not using ROS topics. Instead, it uses XBot2 ways to communicate with Gazebo.

Implementing a new Hal for your robot
###########################################

You must derive the *EEHal* class, and implement the pure virtual sense() and move(). 

In the sense(), you must fill the _js_msg field with the information took by your robot.

In the move(), you must transmit motor position references (that are inside _mr_msg) to your robot.

The *EEHal* base class already implements methods to:

- Propagate *_js_msg* into ROS topics (read by ROS End-Effector main node)
- Receive motor position commands and to fill the *_mr_msg* (there is a ROS callback)

You also must to put the HAL_CREATE_OBJECT(*className*) macro. This is necessary to load a specific HAL at runtime.

You can check DummyHal or XBot2Hal as examples


