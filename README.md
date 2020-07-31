# ROSEndEffectorDocs
The rendered documentation is visible at https://advrhumanoids.github.io/ROSEndEffectorDocs/

This repository contains the source of the documentation of [ROSEndEffector](https://github.com/ADVRHumanoids/ROSEndEffector) package and related packages.

Built with Sphix and readthedocs theme



### To compile locally
- Prerequisite
  ~~~bash
  sudo apt-get install python3-sphinx
  sudo python -m pip install sphinx_rtd_theme
  sudo python3 -m pip install sphinx-copybutton
  ~~~

- Compile locally:
  ~~~bash
  cd [...]/ROSEndEffectorDocs/docs
  make html
  ~~~
