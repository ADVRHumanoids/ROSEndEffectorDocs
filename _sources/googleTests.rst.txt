.. _googleTests:

.. role:: raw-html(raw)
    :format: html

Advanced 
===================================================

This is the developer zone for advanced users.

GTests
##########

We are using Gtest to code maintenance.
:raw-html:`<br />`
Follow the steps to compile and run them on local machine:

- Compile

  .. code-block:: bash
  
    cd <ROSEE_pkg_path>/build
    make tests


- Run Test

  .. code-block:: bash

    #run tests
    make test ARGS="-V" -i #or ctest --verbose
    
 
Code Coverage (from `Arturo Xbot2 <https://github.com/ADVRHumanoids/xbot2_wip/issues/21>`_)
##############################################################################################

*lcov* has been used to see the test coverage.
:raw-html:`<br />`

The html output is available `here(ROS1) <_static/coverage_out/index.html>`_ and `here(ROS2) <_static/coverage_out2/index.html>`_ (please note that it can be not updated)
:raw-html:`<br />`

To run the coverage locally, follow the steps:

- Be sure to have *lcov* installed:

  .. code-block:: bash
  
    sudo apt install lcov  
    
- Then run the coverage:

  .. code-block:: bash
  
    cd build_dir
    ccmake <src_folder> # Check if ROSEE_ENABLE_COVERAGE is set to ON)
    make
    make test_clean_coverage  # cleans old coverage results (they accumulate otherwise!)
    make test ARGS="-V" # run tests in verbose mode
    make test_coverage # generate coverage report
	
Coverage report will be generated in *build_dir/coverage_out/index.html*


Old commands, kept only for reference
##############################################################################################
- Run Test OPTION 2 (Colorful prints but all tests togheter)

  This is old way kept here only for reference, and it should still works anyway. Roscore is runned in
  the test itself so there isn't the need to run external roscore before the tests, or using roslaunch.

  .. code-block:: bash
  
    roslaunch end_effector old_googleTest_run_all.launch 
    
  Check the **old_googleTest_run_all.launch** file to change the hand for the tests
  :raw-html:`<br />`
  :raw-html:`<br />`
  
NOTES about XBOT2 usage
##############################################################################################

- For a new hand, each non fixed joint (even the virtual, passive and mimic) must belong to a chain (convention that  I used is to add a
  virtualXXX named chain, if it is necessary
- Also, all the chains must belong to the supergroup "chains" (name is fixed)
- Check robotiq2f srdf for correctess

- In urdf, add the xbot gazebo plugin. Each joint must have a PID, even the mimic and passive. simply put 0 gains for them. And keep the mimic joint plugin ( maybe in future will be integrated in xbot), it is this one that move the mimic (since for xbot they have 0 gains).
- Again check the robotiq2f urdf as example

- rosee.world has the clock plugin added. This is already done, the world is used by each hand

- Running:
   #. terminal roscore (maybe not necessary)
   #. terminal: source xbot, source rosee, set_xbot2_config (necessary?), and launch rosee_gazebo_plugin (or rosee_startup) file with hand name. RUN gazebo
   #. terminal: set_xbot2_config, export xbot_root, and finally xbot2-core --verbose
   #. terminal: rosservice call /xbotcore/ros_ctrl/switch 1  otherwise command through ros are not read, then you can call the gui to move the hand
  
  - other stuff:
    - To publish via topic, the field ctrl_mode must be set (to 1 for position?)
    - If command is too brute, xbot stops the joint device by default, to restore: rosservice call /xbotcore/joint_master/set_control_mask 1
    



