.. _install:

.. 
  for line break without identation ( | symbol put a identation)
.. role:: raw-html(raw)
    :format: html

Installation
============

ROS End-Effector is a ROS package tested on ROS kinetic with Ubuntu 16.04.
:raw-html:`<br />`
So, first, be sure to have ROS kinetic installed as described `here <http://wiki.ros.org/kinetic/Installation/Ubuntu>`_.

.. note::
  We are going to use the abreviation ROSEE for the ROS End-Effector from now on

Full Installation (recommended)
###############################

The following steps will guide you to install all packages of ROSEE using a *.rosintall* file.
:raw-html:`<br />`
ROS End-Effector is in development. Actually the most updated and working branch is the *devel* one, so you will
clone the "devels" branch of each repository.

.. code-block:: bash

  git clone -b devel https://github.com/ADVRHumanoids/ROSEndEffectorPackageManager.git
  cd ROSEndEffectorPackageManager
  ./setup.sh
  cd src
  catkin_init_workspace
  rosws update
  cd ..
  catkin_make
  
.. note::
   The sh script will install external necessary dependencies with apt-get
   :raw-html:`<br />`

.. note::
  In future, if you want to pull the last updates, you can use :code:`rosws update` to pull all the updates of each repository with this unique command!

You are now ready to use ROS End-Effector! Check :ref:`How to use ROS End-Effector with your end-effector <usage>` and :ref:`Examples with ready to use end-effectors <examples>` sections to learn how to use it. 

ROS End-Effector GUI second tab
********************************

Some functionalities of the ROS End-Effector Graphical User Interface are available only with a *Qt* version greater or equal **5.9**, which may be not installed by default into your system. 
These functionalities include an additional tab where joint state can be plotted in real-time, thanks to *Qt Charts*. More information about the ROS End-Effector GUI are available at :ref:`ROS End-Effector GUI <roseeGui>` page.
:raw-html:`<br />`
Any recent version of *Qt* can be installed following the link to the `Qt website <https://www.qt.io/download-qt-installer/>`_.	

.. note::
	Multiple versions of *Qt* may coexist in your system, so it is recommended to not remove the default version.

When choosing the *Qt* components to install, be sure to check *Qt Charts* from the menu, as in the figure below (installer window may change in newest versions):

.. image:: images/qtInstall.png
  :alt: Qt Installer screen with necessary components selected
  :width: 200
  
.. note::
  With installer, also qtcreator (the IDE) will be installed, without the possibility to uncheck it. This seems a known bug of *Qt* (`reference here <https://bugreports.qt.io/browse/QTBUG-28101>`_). It should be safe to remove qtcreator manually, expecially if you have already it installed.
  
After installed Qt, be sure to compile ROS End-Effector **in an cleaned workspace** (simply delete any *devel*, *build*, *install* folders, if present) specifying the Qt path when calling catkin_make:

.. code-block:: bash

  catkin_make --cmake-args -DQT5_PATH:STRING=#<gcc_64 path folder of qt5>
  
  # for example
  # catkin_make --cmake-args -DQT5_PATH:STRING=/usr/lib/x86_64-linux-gnu/Qt5.12.8/5.12.8/gcc_64
  
.. note::
  the :code:`--cmake-args` argument is only necessary once (when the workspace is cleaned), future calls to :code:`catkin_make` can omit it


Installation issues
#####################  

- Recent version of *Qt* can cause an error like this:
  :code:`qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" ...`
  :raw-html:`<br />`
  Solve simply installing *libxcb-xinerama0* :

  .. code-block:: bash

    sudo apt-get install libxcb-xinerama0  
    
=================================================    

Custom Installation (this could be not updated, kept here mainly for reference)
###################################################################################

If you want, you can clone only the single ROSEE packages, without using the rosintall. 
:raw-html:`<br />`
In this sections there are the steps.

Install External System Dependencies
***************************************

.. code-block:: bash 

  sudo apt-get install ros-kinetic-moveit #moveit
  
  #I do not know if these two are the same...
  sudo apt install ros-kinetic-ros-control
  sudo apt install ros-kinetic-control-toolbox

Install ROS End-Effector package from sources
**************************************************

**IMPORTANT**: ROSEE is a project under development. When cloning, be sure to download the branch you really want. Currently the newest features can be found in the *devel* branch; so, in the commands below substitute <branch_you_want> with devel. Note that also the other ROSEE related packages should be used with the right branch.

The next steps will guide you to the creation of a new catkin_workspace, the cloning of all the necessary repository, and their compilation.

- Create a catkin workspace

  .. code-block:: bash
  
    mkdir ~/ROSEE
    cd ROSEE
    mkdir src
    cd src
    catkin_init_workspace


- Clone Necessary dependencies:

  .. code-block:: bash
   
    git clone -b <branch_you_want> https://github.com/ADVRHumanoids/rosee_msg.git
         
- Clone Optional dependencies:

  - GUI
  
    .. code-block:: bash 
    
      git clone -b <branch_you_want> https://github.com/ADVRHumanoids/rosee_gui.git
  
  - Gazebo Plugin (more info in the dedicated :ref:`section <roseeGazeboPlugins>`)
  
    .. code-block:: bash
    
      git clone https://github.com/roboticsgroup/roboticsgroup_gazebo_plugins.git #necessary external plugin
      git clone -b <branch_you_want> https://github.com/ADVRHumanoids/rosee_gazebo_plugins.git
          
.. note::
  you can see details on each optional plugin in their relative page of this doc
  
- Clone Core Package

  .. code-block:: bash
   
    git clone -b <branch_you_want> https://github.com/ADVRHumanoids/ROSEndEffector
  
- Compile them all!

  .. code-block:: bash
  
    cd ~/ROSEE
    catkin_make    


 

