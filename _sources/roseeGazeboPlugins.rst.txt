.. _roseeGazeboPlugins:

.. role:: raw-html(raw)
  :format: html
    
ROS End-Effector Gazebo Plugins
==================================

This package is an utility to help end effector dynamic simulation with Gazebo. 
It is hosted on *github* `here <https://github.com/ADVRHumanoids/rosee_gazebo_plugins>`_.

Generic Information
#######################

It is very similar to the "official" ROS package for control : `gazebo_ros_control <http://gazebosim.org/tutorials/?tut=ros_control>`_.

It includes a gazebo plugin to take commands (*references*) from **/ros_end_effector/joint_states** topic and use to move the simulated robot in gazebo. This is done with a pid controller. The same plugin also publish the "real" joint states (derived by the gazebo simulation) in the topic **/rosee_gazebo_plugins/joint_states**.

**Take care:** In **/ros_end_effector/joint_states** topic there are states for actuated joints only; while in **/rosee_gazebo_plugins/joint_states** there are states for ALL non-fixed joints (passive and mimic included). This may change in future, but for now I leave as it is because returning also mimic joints is not so a big problem.


At the moment each joint can be controlled in position or in velocity, using the *SetPositionTarget()* and *SetVelocityTarget()* of gazebo. Gains (pid) are settable in a config file (see later for istructions). Please note that for now ROS End-Effector sends only positional references.

In the package, there is an additional ROS node, *DynReconfigure*, which uses the ROS tool *Dynamic Reconfigure server* to give the possibility to change the PID gains during the simulation.


Mimic Joints
**************

Thanks to the `mimic_joint_plugin <https://github.com/roboticsgroup/roboticsgroup_gazebo_plugins>`_ (and to `mimic_joint_gazebo_tutorial <https://github.com/mintar/mimic_joint_gazebo_tutorial>`_ to understand how to use it), now also mimic joints work correctly. The argument *set_pid* of this plugin should be set to false, because otherwise a pid controller is used, that is the "default" gazebo one (see plugin page for more info). And also when mimicking, we want to reply exactly the angle of the other joint (or not?). See above how to deal with this plugin if your urdf model has some mimic joint

How to Install
################

.. note::
  You probably already installed this if you have followed the steps in :ref:`Install <install>` section

.. code-block:: bash

  git clone https://github.com/roboticsgroup/roboticsgroup_gazebo_plugins.git #necessary external plugin
  git clone -b <branch_you_want> https://github.com/ADVRHumanoids/rosee_gazebo_plugins.git
  
  compile with catkin_make


.. _prepare4Gazebo:

Prepare your Model for Gazebo
#################################

You need additional steps on your *.urdf* file to make Gazebo simulation works.

- Be sure to have an urdf file ready for gazebo (info `here <http://gazebosim.org/tutorials/?tut=ros_urdf>`_).
  :raw-html:`<br />`
  Also add in your *.urdf* :
  
  .. code-block:: xml
  
    <gazebo>
      <plugin name="SOMENAME" filename="librosee_plugin.so"> </plugin>
    </gazebo>

  :raw-html:`<br />`

- Add *rosee_gazebo_plugins_args* node in the main *yaml* config file *your_hand.yaml*, that you have created before as instructed in :ref:`Prepare the files <prepareTheFiles>` section. In this node you have a controller for each actuated joint, specifing
  the controller type, the joint which is controlled, and the pid gains.
  An example is below:

  .. code-block:: yaml
  
    ROSEndEffector:
      urdf_path: "urdf/my_hand.urdf"
      srdf_path: "srdf/my_hand.srdf"
    # ^^^^^^^^^^^^^^^^^^^^^^^^^^^^   -- > you should already have these lines
    
    rosee_gazebo_plugins_args:
      
      joint_1_controller_name:
        type: JointPositionController
        joint_name: JOINT NAME
        pid: {p: 0.3, i: 0.001, d: 0.00005}
        
     joint_2_controller_name:
       ...


  - Be sure to put different controller names
  - Supported controllers *type* are *JointPositionController* and *JointVelocityController*, but for now ROS End-Effector only send position reference (so use *JointPositionController* for now).   
    :raw-html:`<br />`
    In truth there exists also *JointEffortController*, we will implement this in future if gazebo will allow it (it is not possible in gazebo 7)
  - The pid gains are settable online thanks to the *DynReconfigure* node. This exploits a ROS tool called *Dynamic reconfigurator server*. The node is inside this package

  :raw-html:`<br />`
  
- **Important**: if your urdf file has some mimic joints, you have to add a `mimic_joint_plugin <https://github.com/roboticsgroup/roboticsgroup_gazebo_plugins>`_ for each one, like below:

  .. code-block:: xml
  
    <joint name="base_to_right_finger" type="revolute">
      <axis xyz="0 0 1"/>
      <limit effort="1000.0" lower="-0.7" upper="0.0" velocity="0.5"/>
      <parent link="base"/>
      <child link="right_finger"/>
      <origin xyz=".1 0 0"/>
      <mimic joint="base_to_left_finger" multiplier="-1" offset="0"/>
    </joint>
    <!-- ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ this is the mimic joint -->
    
    <gazebo>
      <plugin filename="libroboticsgroup_gazebo_mimic_joint_plugin.so" name="base_to_right_finger_jointmimic_joint_plugin">
      <joint>base_to_left_finger</joint>
      <mimicJoint>base_to_right_finger</mimicJoint>
      <multiplier>-1.0</multiplier>
      <offset>0</offset>
      <sensitiveness>0.0</sensitiveness>
      <!-- if absolute difference between setpoint and process value is below this threshold, do nothing; 0.0 = disable [rad] -->
      <maxEffort>1000.0</maxEffort>
      </plugin>
    </gazebo> 
    
    
  This example is taken from *configs/urdf/two_finger_mimic.urdf* file.
  :raw-html:`<br />`
  If you like to use xacro macros to add this plugin, you can create one in this way:
    
    
  .. code-block:: xml
    
    <xacro:macro name="mimic_joint_plugin_gazebo" params="name_prefix parent_joint mimic_joint has_pid:=false multiplier:=1.0 offset:=0 sensitiveness:=0.0 max_effort:=1.0 robot_namespace:=''">
      <gazebo>
      <plugin name="${name_prefix}mimic_joint_plugin" filename="libroboticsgroup_gazebo_mimic_joint_plugin.so">
          <joint>${parent_joint}</joint>
          <mimicJoint>${mimic_joint}</mimicJoint>
          <xacro:if value="${has_pid}">                     <!-- if set to true, PID parameters from "/gazebo_ros_control/pid_gains/${mimic_joint}" are loaded -->
               <hasPID />
          </xacro:if>
          <multiplier>${multiplier}</multiplier>
          <offset>${offset}</offset>
          <sensitiveness>${sensitiveness}</sensitiveness>   <!-- if absolute difference between setpoint and process value is below this threshold, do nothing; 0.0 = disable [rad] -->
          <maxEffort>${max_effort}</maxEffort>              <!-- only taken into account if has_pid:=true [Nm] -->
          <xacro:unless value="${robot_namespace == ''}">
              <robotNamespace>($robot_namespace)</robotNamespace>
          </xacro:unless>
      </plugin>
      </gazebo>
    </xacro:macro>
      
      
  And then use it:
   
  .. code-block:: xml
   
   <xacro:mimic_joint_plugin_gazebo name_prefix="base_to_right_finger_joint"
     arent_joint="base_to_left_finger" mimic_joint="base_to_right_finger"
     has_pid="false" multiplier="-1.0" max_effort="1000.0" />
     
     
  This example is taken from *configs/urdf/two_finger_mimic.urdf.xacro* file.
  :raw-html:`<br />`
  The macro is taken from `mimic_joint_gazebo_tutorial <https://github.com/mintar/mimic_joint_gazebo_tutorial>`_.


How to Run
#############

To run ROS End-Effector as a whole, follow the guide in :ref:`How to use ROS End-Effector with your End-Effector <usage>` section.

- Anyway, ROS End-Effector Gazebo Plugins is a package that run independently, so you can also launch it alone:

  .. code-block:: bash
  
    roslaunch rosee_gazebo_plugins twofinger.launch


- To run the dynamic reconfigurator:
  
  .. code-block:: bash
  
    rosrun rosee_gazebo_plugins DynReconfigure two_finger


- Also useful

  .. code-block:: bash
  
    rqt  
    
  And set it to have things like that, for example to tune the gains:
  
  .. image:: images/rqt.png
    :alt: rqt gui
    :width: 700

   
Advanced
##########

Advanced section only for the braves

Change more params with Dynamic Reconfigurator
************************************************

- Check the ros tutorials about that ( `here <http://wiki.ros.org/dynamic_reconfigure/Tutorials>`_ ) 
- Add (or extend) config files in *rosee_gazebo_plugins/cfg* folder
- Check the DynReconfigure code in *src/DynReconfigure*


