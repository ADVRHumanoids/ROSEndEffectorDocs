.. ROSEndEffector documentation master file, created by
   sphinx-quickstart on Tue Apr 21 14:27:39 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.
.. 
  for line break without identation ( | symbol put a identation)
.. role:: raw-html(raw)
    :format: html


Welcome to ROS End-Effector's documentation!
======================================================


ROS End-Effector package provides a ROS-based set of standard interfaces to command robotics end-effectors in an agnostic fashion.

Documentation about the API of ROS End-Effector can be found at https://advrhumanoids.github.io/ROSEndEffector/index.html


.. toctree::
   :maxdepth: 2
   :caption: Usage
   
   install
   usage
   examples

.. toctree::
   :maxdepth: 2
   :caption: The Main Components
  
   actions
   findActions
   receiveActionsInfo
   newGenericAction
   eeHal
   
.. toctree::
   :maxdepth: 2
   :caption: Further steps  
   
   rosTopics
   mimicJoint  
   
.. toctree::
   :maxdepth: 2
   :caption: Other Packages
  
   roseeGui
   roseeGazeboPlugins
   
   
   
.. toctree::
   :maxdepth: 2
   :caption: Other
   
   issues
   
.. toctree::
   :maxdepth: 2
   :caption: Advanced
   
   googleTests


Package List
=================================================
For reference, here there are listed all the packages involved in this project (links send to github repositories)

- `ROS End-Effector <https://github.com/ADVRHumanoids/ROSEndEffector>`_ - the main package
  :raw-html:`<br />`
  Documentation about the API of ROS End-Effector can be found at https://advrhumanoids.github.io/ROSEndEffector/index.html

  CI powered by Travis CI (.org) 
  :raw-html:`<br />`
  |travisLogo| https://travis-ci.org/ADVRHumanoids/ROSEndEffector
  :raw-html:`<br />`
  :raw-html:`<br />`

- `ROS End-Effector GUI <https://github.com/ADVRHumanoids/rosee_gui>`_ - specific GUI for ROSEndEffector
  :raw-html:`<br />`
  :raw-html:`<br />`

- `ROS End-Effector Gazebo Plugins <https://github.com/ADVRHumanoids/rosee_gazebo_plugins>`_ - gazebo plugins to use ROS End-Effector with Gazebo
  :raw-html:`<br />`
  :raw-html:`<br />`

- `ROS End-Effector msgs <https://github.com/ADVRHumanoids/rosee_msg>`_ - custom ROS messages specific for ROS End-Effector
  :raw-html:`<br />`
  :raw-html:`<br />`

- `ROS End-Effector Documentation <https://github.com/ADVRHumanoids/ROSEndEffectorDocs>`_ - the repository where this documentation is hosted
  :raw-html:`<br />`
  CI powered by Travis CI (.org) 
  :raw-html:`<br />`
  |travisLogoDocs| https://travis-ci.org/github/ADVRHumanoids/ROSEndEffectorDocs
  :raw-html:`<br />`
  :raw-html:`<br />`

- `ROS End-Effector Package Manager <https://github.com/ADVRHumanoids/ROSEndEffectorPackageManager>`_ - metapackage to easy installation
  :raw-html:`<br />`
  :raw-html:`<br />`

.. |travisLogo| image:: https://travis-ci.org/ADVRHumanoids/ROSEndEffector.svg?branch=master
  :target: https://travis-ci.org/ADVRHumanoids/ROSEndEffector
  :alt: Build Status
  :width: 70

.. |travisLogoDocs| image:: https://travis-ci.org/ADVRHumanoids/ROSEndEffectorDocs.svg?branch=master
  :target: https://travis-ci.org/github/ADVRHumanoids/ROSEndEffectorDocs
  :alt: Build Status
  :width: 70
  
Cite our work
=========================
If you find our package useful, please cite our work!

**Towards an Open-Source Hardware Agnostic Framework for Robotic End-Effectors Control**
:raw-html:`<br />`
by Davide Torielli, Liana Bertoni, Nikos Tsagarakis, and Luca Muratore,
:raw-html:`<br />`
*2021 20th International Conference on Advanced Robotics (ICAR)*

.. code-block:: latex

  @INPROCEEDINGS{9659331,
  author={Torielli, Davide and Bertoni, Liana and Tsagarakis, Nikos and Muratore, Luca},
  booktitle={2021 20th International Conference on Advanced Robotics (ICAR)}, 
  title={Towards an Open-Source Hardware Agnostic Framework for Robotic End-Effectors Control}, 
  year={2021},
  volume={},
  number={},
  pages={688-694},
  doi={10.1109/ICAR53236.2021.9659331}}
  
:raw-html:`<br />`
 
  
**Towards a Generic Grasp Planning Pipeline using End-Effector Specific Primitive Grasping Actions**
:raw-html:`<br />`
by Liana Bertoni, Davide Torielli, Yifang Zhang, Nikos Tsagarakis, and Luca Muratore
:raw-html:`<br />`
*2021 20th International Conference on Advanced Robotics (ICAR)*

.. code-block:: latex

  @INPROCEEDINGS{9659402,
  author={Bertoni, Liana and Torielli, Davide and Zhang, Yifang and Tsagarakis, Nikos G. and Muratore, Luca},
  booktitle={2021 20th International Conference on Advanced Robotics (ICAR)}, 
  title={Towards a Generic Grasp Planning Pipeline using End-Effector Specific Primitive Grasping Actions}, 
  year={2021},
  volume={},
  number={},
  pages={722-729},
  doi={10.1109/ICAR53236.2021.9659402}}

=================================================

.. TODO this was a comment
    ROSIN acknowledgement from the ROSIN press kit
    @ https://github.com/rosin-project/press_kit

.. image:: http://rosin-project.eu/wp-content/uploads/rosin_ack_logo_wide.png
  :target: http://rosin-project.eu
  :alt: rosin_logo
  :width: 200
  :align: center

Supported by ROSIN - ROS-Industrial Quality-Assured Robot Software Components.
:raw-html:`<br />`
More information: rosin-project.eu_

.. _rosin-project.eu: https://www.rosin-project.eu/ftp/ros-end-effector


.. image:: http://rosin-project.eu/wp-content/uploads/rosin_eu_flag.jpg
  :alt: eu_flag
  :width: 70
  :align: left 

This project has received funding from the European Union’s Horizon 2020  
research and innovation programme under grant agreement no. 732287. 

