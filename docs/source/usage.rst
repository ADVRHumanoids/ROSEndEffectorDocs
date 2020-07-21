.. _usage:

.. role:: raw-html(raw)
    :format: html

How to use ROS End-Effector with your end-effector
==================================================
This section will guide you through using ROS End-Effector package.

.. _prepareTheFiles:

Prepare the files
####################

ROS End-Effector allows you to control your end-effector with minimal inital set-up, as you will notice in this section :D.

- If you want to use also gazebo and :ref:`ROS End-Effector Gazebo plugin <roseeGazeboPlugins>`, please prepare correctly your *.urdf* model following the steps :ref:`here <prepare4Gazebo>`

- Put *.urdf* and *.srdf* files in *ROSEndEffector/configs/urdf/* and *ROSEndEffector/configs/srdf/* respectively
  (see later for information on how to proper write your *.srdf* file).
  :raw-html:`<br />`
  Be sure that the files are named like the name of the robot tag of *.urdf* and *.srdf*
  
    **E.G.** *my_hand.urdf*, *my_hand.srdf*, with tags in the files like : :code:`<robot name="my_hand" [...] >`

  The meshes can be kept on your hand_description folder, only be sure to :code:`source` that *setup.bash* before run ROSEE nodes.

- Create a yaml config file named *my_hand.yaml* to tell where the *.urdf* and *.srdf* files are, and put it in *ROSEndEffector/configs/* folder. As an example, see below:

  .. code-block:: yaml 

    ROSEndEffector:
      urdf_path: "urdf/my_hand.urdf"
      srdf_path: "srdf/my_hand.srdf"


Creating SRDF files
*********************

Both *moveit* and *ROS End-Effector* refers to SRDF file to explore your hand. So it is important to provide a compatible SRDF file. 
If you do not know the SRDF format, please read `here <http://wiki.ros.org/srdf>`_.

- The convention used by ROS End-Effector is that each finger is a *group*, from the base of the finger to the tip, so in each group you must add a chain, where the *base_link* is the first link of the finger, and the *tip_link* is the last one. For example (taken from *robotiq_3f.srdf*):

  .. code-block:: xml 

    <group name="finger_1">
        <chain base_link="palm" tip_link="finger_1_link_3" />
    </group>

- All the fingers-group must be included in a bigger group, like this:

  .. code-block:: xml 

    <group name="end_effector_fingers">
      <group name="finger_1" />
      <group name="finger_2" />
      <group name="finger_middle" />
   </group>

- In the *SRDF* file you can also indicate some joints as passive in this way:

  .. code-block:: xml 

    <passive_joint name="JOINT_NAME" /> 

  The passive tag is used to indicate to ROS End-Effector that a non-fixed joint is not actuated (i.e. it can move only if some external forces are applied, in a compliant way). If you not put this tag, the joint will be considered actuated.
  Please **DO NOT** add the passive tag for joints that are defined as mimic in the urdf. This can cause problem and it is useless, because mimic joints are already considered as non-actuated by ROSEE. 
  :raw-html:`<br />` 
  Also, it is not necessary to set the passive tag for fixed joints.
	
- If you are planning to use gazebo, specify as *virtual* the joint between the *world* and the end-effector base is necessary, like below:

  .. code-block:: xml 

    <virtual_joint name="world_base" type="fixed" parent_frame="world" child_link="palm" />


Even for very complicated hand, this file is easy to create (for example, see the one for the *Schunk SVH* `here <https://github.com/ADVRHumanoids/ROSEndEffector/blob/devel/configs/srdf/schunk.srdf>`_).

If you don't want to create this by hand, you can use the `moveit setup assistant <http://docs.ros.org/kinetic/api/moveit_tutorials/html/doc/setup_assistant/setup_assistant_tutorial.html>`_ , which will help to create SRDF files (among the other things) through a GUI. Only be sure to create the group as *chain* (use *add_chain* and not *add_joint* not *add_link*)

.. _offPhase:

Find Grasping Actions (offline phase)
######################################
Before control a new end-effector, the first step is to let **UniversalFindActions** explore your model and extract the grasping primitive actions. So, run the dedicated launch:

  **WARNING**: old *yaml* files of grasping actions will be ovewritten every time you run again this node.
  :raw-html:`<br />`
  **NOTE**: As you may known, :code:`roslaunch` starts :code:`roscore` it is not already running; so when findActions is finished, :code:`roscore` may continue running in the terminal. It is safe to :code:`ctrl+c` in the terminal as soon it prints something like : :code:`[UniversalFindActions-2] process has finished cleanly`.


.. code-block:: bash

  # source the setup.bash of package where robot meshes are
  roslaunch ros_end_effector findActions.launch hand_name:=my_hand


  
The findActions node will generate yaml files ( in the *config/action/my_hand/* folder ) for the grasping primitives extracted from your robot's model. 
These files will be parsed each time you run the main node (as explained in the next section).
:raw-html:`<br />`
To know more about this part, and how to create custom grasping actions, see :ref:`ROS End-Effector Grasping Actions <actions>` and :ref:`FindActions <findActions>` sections

.. _controlEEWithROSEE:

Control your End-Effector with ROSEE (online phase)
####################################################

The main node of ROS End-Effector will wait for grasping actions command (received as *ROS actions*) and it will send low level commands (such as joint position references) to the end-effector in use.
:raw-html:`<br />`
To launch this, please run:

.. code-block:: bash

  # source the setup.bash of package where robot meshes are
  roslaunch ros_end_effector rosee_startup.launch hand_name:=my_hand
  
This command will load ROSEE controller node (called *UniversalROSEndEffector*) with rviz for visualization purposes.

  **Note**: If you want to not run ROSEE controller but load the ROS joint publisher GUI to command directly each joint position, you can simply run :code:`roslaunch ros_end_effector jsp_startup.launch hand_name:=my_hand` . This can be useful to visualize the end-effector and try to move it setting joints positions thanks to ROS tools.

In another terminal, you can run the ROS End-Effector GUI to command the grasping actions. Only be sure to have *rosee_gui* package installed (that you should have if you have followed :ref:`Installation <install>` section).
:raw-html:`<br />`
To run the GUI, please run:

.. code-block:: bash

  roslaunch rosee_gui gui.launch #no hand name is needed

This will load the GUI dynamically, visualizing only the grasping action defined for the end effector in use.
:raw-html:`<br />`
To know more about ROSEE GUI, check :ref:`ROS End-Effector GUI <roseeGui>` section.

Publishing grasping action command without GUI
************************************************

The other way to command grasping actions is directly publishing on ROS topics (that is what GUI does for you). 
You can simply :code:`pub` a *rosee_msg/ROSEECommandActionGoal* message on */ros_end_effector/action_command/goal* topic, and :code:`echo` on /*ros_end_effector/action_command/feedback* to receive the feedback.

For example, to publish run this:

.. code-block:: bash
  :emphasize-lines: 13,14,15,16,17,18,19,20
  
  rostopic pub /ros_end_effector/action_command/goal rosee_msg/ROSEECommandActionGoal "header:
    seq: 0
      stamp:
        secs: 0
        nsecs: 0
    frame_id: ''
  goal_id:
    stamp:
      secs: 0
      nsecs: 0
    id: ''
  goal:
    goal_action:
      seq: 0
      stamp: {secs: 0, nsecs: 0}
      action_name: 'pinchTight'
      action_type: 0
      actionPrimitive_type: 0
      selectable_items: ['index', 'thumb']
      percentage: 1.0" 

**NOTE** the important lines are the last ones.

And to receive feedback, run (in another terminal):

.. code-block:: bash
  
  rostopic echo /ros_end_effector/action_command/feedback 



Dynamic Simulation with Gazebo
********************************

Be sure to have installed the *rosee_gazebo_plugins* package (that you should have if you have followed :ref:`Installation <install>` section). Be also sure that and your urdf model is ready to be used with Gazebo, as explained :ref:`Prepare your Model for Gazebo <prepare4Gazebo>` section. 

  **Note** Also remember that you have to run the :ref:`offline phase <offPhase>` once if you have never run it for your end-effector.

Launch the main node with the :code:`gazebo:=true` argument, in this way:

.. code-block:: bash

  # source the setup.bash of package where robot meshes are
  # if some errors happens, try to also source gazebo.setup, like "source /usr/share/gazebo/setup.sh"
  roslaunch ros_end_effector rosee_startup.launch hand_name:=my_hand gazebo:=true
  
As before, you can use the ROSEE GUI to send grasping action commands, running in another terminal:

.. code-block:: bash

  roslaunch rosee_gui gui.launch #no hand name is needed



