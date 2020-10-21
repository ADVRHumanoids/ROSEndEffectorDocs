.. _newGenericAction:

.. role:: raw-html(raw)
  :format: html

How to add a new Generic action in the system
===================================================

When ROSEE main node is running, you may want to introduce a new action in the system, such that you can command it afterwards. This is possible through a dedicated ROS service.

.. note::
  The action will belong to the *generic* category

The request is composed as following:

.. code-block:: sh

	## Service associated with GenericGraspingAction message

	NewGenericGraspingAction newAction

	# user can also request to store the action in a yaml file, togheter with the others
	bool emitYaml

	---

	# the service will give true if action is good and it is inserted in the structure
	bool accepted

	# true false depending if emission on yaml file is successful
	bool emitted

	# string containing useful info about a possible not acceptance
	string error_msg

Where NewGenericGraspingAction is defined as: 

.. code-block:: sh

	# Message which include all the infos necessary to insert a new action in the system 
	# (https://github.com/ADVRHumanoids/ROSEndEffector/issues/71)

	# name of the action
	string action_name

	# Motor Position
	MotorPosition action_motor_position

	# OPTIONAL The mask which says if the motor is used by the action (count > 0) or not (count = 0).
	# take care that if it is not included, the not used joint will be considered the ones with 0 position
	JointsInvolvedCount action_motor_count
		                                
	# OPTIONAL field to indicate which hand element will be moved by the action
	string[] elements_involved


Hence you can call the service from usual c++ code or also from terminal for example:

.. code-block:: bash

	rosservice call /ros_end_effector/new_generic_grasping_action "newAction:
		action_name: 'newFancyAction'
		action_motor_position:
		  name:
		  - 'joint_1'
		  - 'joint_2'
		  position:
		  - 1
		  - 2
	emitYaml: true" 

.. warning::

  Some checks are performed when calling the action (eg no action with the same name must exists, consistency among motor position and motor count...) 
  but NO CHECKS are performed to see if the name of the joints are the correct ones. Remember to put ALL the motors in the action_motor_position field!
	


