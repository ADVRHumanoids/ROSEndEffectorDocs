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


- Run Test OPTION 1 (suggested)

  Multiple tests on multiple end-effectors will be launched. 
  Check the cmake to change/add the end-effectors for the tests
   
  .. code-block:: bash
    
    make test ARGS="-V" #or ctest --verbose
 
Code Coverage (from `Arturo Xbot2 <https://github.com/ADVRHumanoids/xbot2_wip/issues/21>`_)
##############################################################################################

*lcov* has been used to see the test coverage.
:raw-html:`<br />`

The html output is available `here <_static/coverage_out/index.html>`_ (please note that it can be not updated)
:raw-html:`<br />`

To run the coverage locally, follow the steps:

- Be sure to have *lcov* installed:

  .. code-block:: bash
  
    sudo apt install lcov  
    
- Then run the coverage:

  .. code-block:: bash
  
    cd build_dir
    ccmake . # Check if ROSEE_ENABLE_COVERAGE is set to ON)
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
  
    roslaunch ros_end_effector old_googleTest_run_all.launch 
    
  Check the **old_googleTest_run_all.launch** file to change the hand for the tests
  :raw-html:`<br />`
  :raw-html:`<br />`
  

    



