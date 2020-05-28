.. _rosTopics:

.. role:: raw-html(raw)
  :format: html
  
ROS Topics Structures
========================

This section is an aid to discricate among the ros topics involved in the framework.
:raw-html:`<br />`
You can visualize the topics as in the following images at any time during while the framework is running, using
the ROS tool :code:`rqt_graph`.
:raw-html:`<br />`
As reminder, in :code:`rqt_graph` the ovals are the ROS nodes, the inner rectangles are the topics, the outer ones
are the namespaces (which the inner topics belong), and the parallelepipeds are for ROS action (that are a sort of collections of topics). The arrows indicate ROS publications and subscriptions.

Kinematic Simulation Only
###########################

.. image:: images/rqt_graph.png
  :alt: ros topics kinematic simulation only
  :width: 700
    
- **gui_main**
  is the ROS End-Effector GUI node, which sends commands. The GUI can be substitutes with publishers and
  subscribers as briefly explained in :ref:`controlEEWithROSEE` section.
  :raw-html:`<br />`
  The GUI publish actions to */ros_end_effector/action_command/action_topics* and receives feedbacks from the same   
  action

- **UniversalRosEndEffector** receive commands from */ros_end_effector/action_command/action_topics*,
  interpret them, and, if they are correct, send the joint position reference (for only actuated joints) to */ros_end_effector/joint_commands*

- `joint_state_publisher <http://wiki.ros.org/joint_state_publisher>`_ is a standard ROS node to "convert" joint_commands into joint_states. The difference is that joint_commands contain only positions for the *actuated* joints, while for the simulation we want also that the `mimic joints <http://wiki.ros.org/urdf/XML/joint>`_ move accordingly. This node is obviously only necessary when simulating.

- `robot_state_publisher <http://wiki.ros.org/robot_state_publisher>`_ simply "convert" joint_states into */tf*, necessary for rviz visualization
    
    
Dynamic simulation (with gazebo)
#########################################

.. image:: images/rqt_graph_gaz.png
  :alt: ros topics kinematic gazebo simulation
  :width: 700
    
With the dynamic simulations the graph is similar. The only difference is that, instead of the **joint_state_publisher**, there is the **gazebo** node. Inside this node, it is present the :ref:`rosee_gazebo_plugins <roseeGazeboPlugins>` (which is not a standalone node because it is only a "plugin" for gazebo node). The plugin's job includes to consider the mimic joints, similarly to what the **joint_state_publisher** did before.
:raw-html:`<br />`
The */joint_states* topic has now a different namespace (i.e. */rosee_gazebo_plugins*) to remember us that we are using gazebo, but its messages are of the same type as before (i.e. *js_publisher*).

