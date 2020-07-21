.. _examples:

Examples with ready to use end-effectors
=========================================

In this section, there are guidelines on how to run some examples with tested end-effectors.

In the ROS End-Effector packages, there are some simple geometrical grippers created only for showing purposes:

- test_ee
- two_finger
- two_finger_mimic

To try examples with these grippers, you need to run *UniversalFindActions* and *UniversalROSEndEffector* nodes as described in :ref:`How to use ROS End-Effector with your End-Effector <usage>` section. We report here the necessary commands:

.. code-block:: bash

  #substitute test_ee accordigly
  
  roslaunch ros_end_effector findActions.launch hand_name:=test_ee #for the offline phase
  roslaunch ros_end_effector rosee_startup.launch hand_name:=test_ee #for the online phase
  

We have also tested the framework with real end-effectors' model. To test them, you need the meshes, so you need to install their specific packages. See below the instructions on how to download them.

Until now, the tested end-effectors are:

- `HERI II`_
- `Schunk SVH 5-finger hand`_
- `qb SoftHand`_
- `Robotiq 3-Finger Gripper`_
- `Robotiq 2F-140 Gripper`_


.. _`HERI II`: 
 
HERI II
**************

The files for this hand are already in the ROS End-Effector main package. So simply run:

.. code-block:: bash

  roslaunch ros_end_effector findActions.launch hand_name:=heri_II #offline phase
  roslaunch ros_end_effector rosee_startup.launch hand_name:=heri_II #online phase

Remember to add :code:`gazebo:=true` to the second command if you want to launch the dynamic simulation with gazebo.


.. _`Schunk SVH 5-finger hand`:

Schunk SVH 5-finger hand
***************************

This is a complex human-like end-effector with lot of actuted joints and DOFs. More details about it can be found 
in `Schunk website <https://schunk.com/it_en/gripping-systems/highlights/svh/>`_.

Some packages are necessary to run ROS End-Effector with this gripper. Follow the instructions to download and compile them:

.. code-block:: bash

  mkdir ~/schunk_ws
  mkdir ~/schunk_ws/src
  cd ~/schunk_ws/src
  git clone https://github.com/fzi-forschungszentrum-informatik/fzi_icl_core.git #schunk library, I am not sure if needed for only the simulation
  git clone https://github.com/fzi-forschungszentrum-informatik/fzi_icl_comm.git #schunk library, I am not sure if needed for only the simulation
  git clone https://github.com/fzi-forschungszentrum-informatik/schunk_svh_driver.git #the main schunk repo
  cd ..
  catkin_make_isolated
  source devel_isolated/setup.bash   

Then run the nodes as usually but with :code:`hand_name:=schunk` as argument.

  **Note**: the SVH's URDF does not have dynamic parameters, so at the moment it can not be simulated with gazebo (so do not use :code:`gazebo:=true`).
  
  
.. _`qb SoftHand`: 
 
qb SoftHand
*****************

This end-effector has a humanoid structure but it has only a single actuated joint. It can close all the fingers together in a compliant way. More information can be found at `qb robotics website <https://qbrobotics.com/products/qb-softhand-research/>`_.

Necessary steps before running ROS End-Effector with this hand:

.. code-block:: bash

  mkdir ~/qbhand
  cd ~/qbhand
  mkdir src
  cd src
  git clone --branch production-kinetic https://bitbucket.org/qbrobotics/qbdevice-ros.git
  git clone --branch production-kinetic https://bitbucket.org/qbrobotics/qbhand-ros.git
	cd ~/qbhand
  catkin_make
  source devel/setup.bash
  
Then run the nodes as usually but with :code:`hand_name:=qbahnd` as argument.


.. _`Robotiq 3-Finger Gripper`:
  
Robotiq 3-Finger Gripper
**************************

This is a gripper with 3 fingers and two actuators. The first one close all the fingers, the other one spread the two adiacent fingers. More information can be found in the `Robotiq website <https://robotiq.com/products/3-finger-adaptive-robot-gripper/>`_.

Necessary steps before running ROS End-Effector with this gripper:

.. code-block:: bash

  mkdir ~/robotiq_ws
  cd ~/robotiq_ws
  mkdir src
  cd src
  git clone https://github.com/ros-industrial/robotiq.git
  cd robotiq
  git checkout kinetic-devel
  cd ../..
  rosdep update
  rosdep install robotiq_modbus_tcp
  sudo apt-get install ros-kinetic-soem
  rosdep install --from-paths src/ --ignore-src --rosdistro kinetic
  catkin_make
  source devel/setup.bash
  
Then run the nodes with :code:`hand_name:=robotiq_3f` as argument  

  **Note** The original urdf from robotiq has been modified a bit. First, some joints have been put as mimic. Then, friction and damping of joints parameters have been inserted so the model could be used in gazebo. Other addition are contact coefficents (of tips) and colors. These parameters obviosly can be very different from the real ones.


.. _`Robotiq 2F-140 Gripper`:

Robotiq 2F-140 Gripper
**************************

This is an industrial parallel gripper with two pads and a single actuator to close them.
More information can be found in the `Robotiq website <https://robotiq.com/products/2f85-140-adaptive-robot-gripper/>`_.

Necessary files are in the same repository of :ref:`Robotiq 3-Finger Gripper` so follow those steps if you did not do it before. 

You can try this gripper as usual running the nodes with :code:`hand_name:=robotiq_2f_140` as argument. 

