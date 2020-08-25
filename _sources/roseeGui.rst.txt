.. _roseeGui:

ROS End-Effector GUI
==================================

ROS End-Effector has a dedicated Graphical User Interface to communicate with the framework, sending grasping actions through *ROS topics*. The GUI is loaded dynamically at run-time, hence it will be different for each end-effector loaded. For example, it will show only the grasping actions specific for the end-effector in use. It also permits to monitor the joints states directly in its window.

.. figure:: images/schunkGui.png
  :alt: GUI for Schunk SVH hand
  :width: 600
  :align: center	
  
  ROSEE GUI for Schunk SVH hand
  
.. figure:: images/heriGui.png
  :alt: GUI for HERI II hand
  :width: 600
  :align: center
  
  ROSEE GUI for HERI II hand
  
The GUI has also a second tab, where the robot states can be plotted in real time. This functionality is a fork of `robot_monitoring <https://github.com/ADVRHumanoids/robot_monitoring>`_.

.. figure:: images/schunkgui2.png
  :alt: GUI for Schunk SVH hand, 2nd tab
  :width: 600
  :align: center
  
  The GUI second tab for the Schunk SVH hand
  
.. figure:: images/herigui2.png
  :alt: GUI for HERI II hand, 2nd tab
  :width: 600
  :align: center
  
  The GUI second tab for the HERI II hand

The GUI package is hosted on *github* `here <https://github.com/ADVRHumanoids/rosee_gui>`_. 


How To Install
################

.. note::
  You probably already installed this if you have followed the steps in :ref:`Install <install>` section. If you have followed the recommended installation, skip this section.

ROS End-Effector is based on `Qt <https://www.qt.io/>`_. The minimum version is Qt5, but for the second tab with the real time plots at least *Qt 5.9* is required.

.. code-block:: bash

  git clone -b <branch_you_want> https://github.com/ADVRHumanoids/rosee_gui
  
  compile with catkin_make


========================================================================================

  
Advanced
##########

How it works - code structure (this could be not updated, kept for reference)
*********************************************************************************

- **main.cpp** : It handles ROS (creating the nodehandle) and the Qapplication. It creates the Window object
- **Window.cpp** a *QWidget* derived class which refers to the gui Window. It has as member a *QGridLayout*, and it creates all the inner *QGridLayout* (one for each action)
- **ActionLayout.cpp** *QGridLayout* derived class, which contain labels buttons and other widgets. It also send message to ros topic, handling a ros publisher with a nodehandle passed to its costructor
- **ActionBoxesLayout.cpp** Derived class from above, it includes checkboxes to select finger/joint and send them also with topics. 
- **ActionTimedLayout.cpp**, **ActionTimedElement.cpp** Container and element for the timed action, which per definition is composed by other actions executed one after another with some time margins. Each element has three progress bar so in the future we can take feedback and display a progress of the action execution, included time margins


Developers note
*****************
Compiling
The default qt installer is a gui, we cant use it on travis. I found by chance this `aqtinstall <https://github.com/miurahr/aqtinstall/>`_, maybe it can be useful in future
