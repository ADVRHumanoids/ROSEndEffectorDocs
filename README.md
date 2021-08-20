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
  
### Troubleshoot
If something like : 
```
make html
sphinx-build -b html -d build/doctrees   source build/html
Running Sphinx v4.1.2
WARNING: sphinx_rtd_theme (< 0.3.0) found. It will not be available since Sphinx-6.0

Theme error:
no theme named 'sphinx_rtd_theme' found (missing theme.conf?)
Makefile:56: recipe for target 'html' failed
make: *** [html] Error 2
```
happens, I solved installing the rtd_theme also for python3:
```
pip3 install sphinx-rtd-theme
```
