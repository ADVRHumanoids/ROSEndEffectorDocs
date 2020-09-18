.. _receiveActionsInfo:

.. role:: raw-html(raw)
  :format: html

How to retrieve the Grasping Action Created
===================================================

Here we will see how it is possible to retrieve information about all the action created for a specific end-effector.

All the Grasping Actions are stored in .yaml file, and ROS End-Effector provides functionalities to easily access them, without the need to write code to parse them.
:raw-html:`<br />`
There are two ways to retrieve grasping action information. 

:ref:`One<Retrieving-Grasping-Actions-with-ROS-Service>` is by means of `ROS Services <http://wiki.ros.org/Services>`_,. For this is necessary that ROS End-Effector is in execution (being the *server* which will respond to the request).

The :ref:`other<Retrieving-Grasping-Actions-with-MapActionHandler>` is by using the ROS End-Effector library *MapActionHandler*, an utility class which parse the yaml files and store the actions in more comfortable containers (*std::map*). This is the method which ROS End-Effector itself uses to parse the files.


.. _Retrieving-Grasping-Actions-with-ROS-Service:

Retrieving Grasping Actions with ROS Service
###############################################

First, be sure that ROS End-Effector is in execution (as explained in :ref:`Control your End-Effector with ROSEE <controlEEWithROSEE>`). 
:raw-html:`<br />`
Be also sure that the service named **ros_end_effector/grasping_actions_available** by default (the name can be changed in the rosee_startup launch file) is available with :code:`rosservice list`.

In the request, the fields are:

- *action_type* field: (note that if not filled it would be 0)

  - 0 : Primitives)
  - 1 : Generic and Composed
  - 2 : Timed


- *primitive_type* field, only considered if *action_type* is 0. Also note that if this is not 0, the field *action_name* is not considered:

  - 0 : All Primitives. If also the action_name field is empty, all primitives will be given in the response
  - 1 : PinchTight
  - 2 : PinchLoose
  - 3 : MultiplePinchTight
  - 4 : Trig
  - 5 : TipFlex
  - 6 : FingFlex
  - 7 : SingleJointMultipleTips
  - 8 : None (not used)

- *action_name* field, the name of the action, it can be used for all *action_type*

- *elements_involved* field, used only for primitives, if action_name is not an empty string and/or primitive_type != 0. This is a vector which can contain
  the element which distinguish a primitive, like 2 fingers for a PinchTight


The respond is a vector of grasping actions (that can be also empty or with only one element)

You can use :code:`rosservice call` command to call the service:

.. code-block:: bash

  rosservice call /ros_end_effector/grasping_actions_available "action_type: 0
  primitive_type: 1
  action_name: ''
  elements_involved:
  - 'thumb' 
  - 'index'" 

This command will request the primitive (*action_type* : *0*) of type PinchTight (*primitive_type* : *0*) done with the *thumb* and *index* fingers.

You can check the GraspingActionsAvailable.srv and GraspingAction.msg files (from rosee_msg package) 

 
.. _Retrieving-Grasping-Actions-with-MapActionHandler:

Retrieving Grasping Actions with *MapActionHandler*
######################################################

**TODO**


