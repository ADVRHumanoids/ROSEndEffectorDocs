.. _googleTests:

.. role:: raw-html(raw)
    :format: html

How to run the google tests present in the package
===================================================

This is the developer zone for advanced users.

- Compile

  .. code-block:: bash
  
    cd <ROSEE_pkg_path>/build
    make tests


- Run Test OPTION 1 (suggested)

  Multiple tests on multiple end-effectors will be launched. 
  Check the cmake to change/add the end-effectors for the tests
   
  .. code-block:: bash
    
    make test ARGS="-V" #or ctest --verbose
      
    
- Run Test OPTION 2 (Colorful prints but all tests togheter)

  This is old way kept here only for reference, and it should still works anyway. Roscore is runned in
  the test itself so there isn't the need to run external roscore before the tests, or using roslaunch.

  .. code-block:: bash
  
    roslaunch ros_end_effector old_googleTest_run_all.launch 
    
  Check the **old_googleTest_run_all.launch** file to change the hand for the tests
  :raw-html:`<br />`
  :raw-html:`<br />`
  

    



