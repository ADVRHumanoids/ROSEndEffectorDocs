.. _issues:

.. role:: raw-html(raw)
    :format: html

Possible Issues
========================

Generic
#########

- From 28-01-2020 the *use_gui* param (in launch files) gives an error because it is deprecated. This causes the sliders of joint state publisher not shown. To solve : 

  .. code-block:: bash
  
    sudo apt install ros-kinetic-joint-state-publisher-gui

.. note::
  *use_gui* is not used anymore by us, but this answer can be useful anyway

- With the schunk hand, if we move only the middle finger (base phalange)
  toward the hand, a collision between index tip, middle tip and ring tip is detected. 
  Easy reproducible with the `moveit assistant <http://docs.ros.org/kinetic/api/moveit_tutorials/html/doc/setup_assistant/setup_assistant_tutorial.html>`_, in the set pose section (when we move the middle finger it finds a collision when visually is not present)
  I dont know if it is a problem of schunk model, moveit, or both.
  :raw-html:`<br />`
  :raw-html:`<br />`
  
Rviz problems
##############

- Rviz does not visualize my model!

  - Be sure to have :code:`source devel/setup.bash` the package with robot meshes
    :raw-html:`<br />`
    :raw-html:`<br />`
  
  
  - Be sure to have set the right *Fixed Frame* in Global Options (usually it is called *base_something*, or *world*
    if you have set up your *urdf* file for Gazebo usage.
    :raw-html:`<br />`
    :raw-html:`<br />`
    
  - Be sure to have added the RobotModel (it should be present in the right menu)

Gazebo explosions
##################

- Gazebo dynamic simulation problems
  
  If gazebo simulation is unstable, or the model explodes, or other similar issues happen, the reasons can be many.

  - Be sure that dynamic parameters of the model are correct. In particular, in the urdf file, other than the :code:`<inertial>` tags, you will probably need also some dynamic params for joints: 

    .. code-block:: xml
    
      <dynamics damping="SOMEVALUE" friction="SOMEVALUE"/>
  
    If you not put these values, or they are too low (like <10e-05?) you could have some issues. They may be caused by some divisions by 0 (or a very low number) that cause too high magnitudes somewhere in the computations. Even if you do not known the real parameters, the experiments should not be influenced so much by wrong values.
  
    :raw-html:`<br />`
  - Similar problems can be caused by too high pid gains. They should be set accordingly *also* to these dynamics values, so more trials may be necessary. You can check the files for the tested hands (present in the config folder) to have an idea of magnitudes that do not cause problems on the specific end-effectors.	     
     
