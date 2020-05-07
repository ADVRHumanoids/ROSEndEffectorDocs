.. _findActions:


FindActions
=====================================

**TODO** put link to api doc for the classes

Primitives
################

The **FindActions** class will help you to extract automatically the primitive actions that the loaded end-effector can accomplish. Each kind of primitive has its own method to understand if a primitive is doable and how (i.e. which joints moves and in which position).

In brief, the pinch actions (PinchTight, PinchLoose, MultiplePinchTight) are found looking for collisions between the fingertips. A high number of random end-effector configurations (i.e. joint positions) is explored, and the configurations where the *best* collisions are found are stored. This is done with the help of some *Moveit* functionalities.
The other kind of primitives are found exploring the kinematic tree of the models. For example, a trig is found selecting all the joints of a finger (if some are present).

The selected configurations for each primitive are then stored in classes. The classes themselves will be in charge of emitting the yaml file (one file per primitive), that can be parsed during the online phase. Also the parsing is done with the methods of the primitives.

In the following part, we will explore the *UniversalFindActions* node, to understand how to use the **FindActions** class. The complete code is avaiable at (TODO put link). Here only the essential parts are descripted.

First, we need to init the ros node, fill the parameter with the urdf and srdf file (this is necessary because Moveit reads these files from there):

.. code-block:: c++

  ros::init ( argc, argv, "FindActions" );
  ros::NodeHandle nh;
  
  ROSEE::Parser parser(nh);
  parser.init();
  
  nh.setParam("robot_description", parser.getUrdfString());
  nh.setParam("robot_description_semantic", parser.getSrdfString());


A special class, *ParserMoveit* is in charge of exctrating information from the model, so we need to init it prior to pass it to the FindActions class:

.. code-block:: c++

  std::shared_ptr <ROSEE::ParserMoveIt> parserMoveIt = std::make_shared <ROSEE::ParserMoveIt> ();
  
  if (! parserMoveIt->init ("robot_description") ) {
    ROS_ERROR_STREAM ("FAILED parserMoveit Init, stopping execution");
    return -1;
  }  
    
  ROSEE::FindActions actionsFinder (parserMoveIt);


Now we can call the various find**** to look for the primitives. These functions will return the actions as c++ maps, but also they will create the yaml files and fill them.

.. code-block:: c++

  auto maps = actionsFinder.findPinch(folderForActions + "/primitives/");

  std::map <std::string, ROSEE::ActionTrig> trigMap =  actionsFinder.findTrig (ROSEE::ActionPrimitive::Type::Trig,  folderForActions + "/primitives/") ;

  std::map <std::string, ROSEE::ActionTrig> tipFlexMap = actionsFinder.findTrig (ROSEE::ActionPrimitive::Type::TipFlex, folderForActions + "/primitives/");

  std::map <std::string, ROSEE::ActionTrig> fingFlexMap = actionsFinder.findTrig (ROSEE::ActionPrimitive::Type::FingFlex, folderForActions + "/primitives/");
  
  unsigned int nFinger = 3;
  std::map < std::string, ROSEE::ActionSingleJointMultipleTips> singleJointMultipleTipsMap = actionsFinder.findSingleJointMultipleTips (nFinger, folderForActions + "/primitives/") ;
    
  nFinger = 2;
  std::map < std::string, ROSEE::ActionSingleJointMultipleTips> singleJointMultipleTipsMap2 = actionsFinder.findSingleJointMultipleTips (nFinger, folderForActions + "/primitives/") ;
    
  auto mulPinch = actionsFinder.findMultiplePinch(3, folderForActions + "/primitives/" );  


As said, each find**** will return the action from where information about the primitive can be get. But, in general, we want to call the find**** only once during the offline phase, and then parse the emitted yaml files during the initialization of the online phase.

So, in the same file, there are examples on how these action can be parsed.
First, a MapActionHandler must be created. This is a support class that internally can parse all primitives (or only some, if wanted):

.. code-block:: c++

  ROSEE::MapActionHandler mapsHandler;
  mapsHandler.parseAllPrimitives(folderForActions + "/primitives/");
  

We can then call mapsHandler methods to take a single action. We can take a primitive both by its name (e.g. "pinchTight"), or by its type (e.g. ROSEE::ActionPrimitive::Type::Trig):

.. code-block:: c++

  auto pinchTightParsed = mapsHandler.getPrimitiveMap("pinchTight");

  auto trigParsed = mapsHandler.getPrimitiveMap(ROSEE::ActionPrimitive::Type::Trig);


If the action is not found (i.e. the yaml file is not present) the function simply return an empty map and print an error.
The returned map are of type :code:`typedef std::map < std::set < std::string >, ROSEE::ActionPrimitive::Ptr > ActionPrimitiveMap`.
The key contains one or more string to select the specific primitive among the one of same type. For example, for a tightPinch, each key is a set containing two strings: the names of the fingers that colllide.
For example:

.. list-table:: TightPinch Example
   :widths: 20 20
   :header-rows: 1

   * - Key
     - Value
   * - [Thumb, Index]
     - The TightPinch primitive which makes thumb and index to collide
   * - [Thumb, Ring]
     - The TightPinch primitive which makes thumb and ring to collide
     
The number of element in the set depends obviously on the type of the primitive, for example a trig has only one element, the multiPinchTight_3 will have three elements.

The value is a pointer to ActionPrimitive class, which has some simply get methods to extract the essential information about the action, for example :code:`getJointPos()`


Generic Actions (the custom ones)
#####################################

The same file also shows how it is possible to create custom actions in the code. 
We can create a custom action *summing* two previously created actions (primitives, or other composed actions), thanks to the :code:`sumAction()` function. This function simply combine the actions **summing** the joint positions:

.. code-block:: c++

  if (mapsHandler.getPrimitiveMap(ROSEE::ActionPrimitive::Type::Trig).size() > 0  &&  
      mapsHandler.getPrimitiveMap(ROSEE::ActionPrimitive::Type::Trig).at(0).size() == parserMoveIt->getNFingers() ) {
        
    std::cout << "A composed action with Independent inner action: " << std::endl;
    ROSEE::ActionComposed grasp ("grasp", true);
        
    for (auto trig : mapsHandler.getPrimitiveMap("trig")) {
      grasp.sumAction  (trig.second) ; 
    }
    grasp.print();

If we want to emit the yaml file about this action, we are now us in charge of call the right function:

.. code-block:: c++

  yamlWorker.createYamlFile (&grasp, folderForActions + "/generics/");
        
        
We can then parse all the generics with the mapsHandler used before for the primitives:

.. code-block:: c++
        
  mapsHandler.parseAllGenerics (folderForActions + "/generics/");
        
  std::cout << "PARSED COMPOSEd" << std::endl;
  mapsHandler.getGeneric("grasp")->print();


Note that the above example creates a :code:`ActionComposed` object, namely a class that is created *summing* two or more actions. We can also create a generic action from scratch, manually filling the essential structures

So, first we fill the ROSEE::JointPos and ROSEE::JointsInvolvedCount structures. In the below example, we will copy inside them information from other actions:

.. code-block:: c++

  ROSEE::JointPos jp;

  //for now copy jp of another action
  jp = ROSEE::operator*(maps.first.begin()->second.getJointPos(), 2);
  auto jpc = maps.first.begin()->second.getJointsInvolvedCount();

  ROSEE::ActionGeneric simpleAction("casual", jp, jpc);

  //always remember to emit it...
  yamlWorker.createYamlFile( &simpleAction,  folderForActions + "/generics/" );

  mapsHandler.parseAllGenerics (folderForActions + "/generics/"); //NOTE already called before

  std::cout << "The parsed casual: " << std::endl;
  mapsHandler.getGeneric("casual")->print();


You can also fill the structure inserting key+value in the map. The ROSEE::JointsInvolvedCount is a 
:code:`typedef std::map <std::string, unsigned int> JointsInvolvedCount`, where the value must be 0 or 1: 1 if the joint is used in the action, 0 otherwise. The key is the name of the joint.

**It is important** to have all actuated joints as keys in both ROSEE::JointsInvolvedCount and ROSEE::JointPos



Timed Actions
##############

These kind of actions are created similarly to generic ones. The difference is that here we execute the action that we add *one after the other*, so we do not *sum* the joint positions. An example is always in the same file:

.. code-block:: c++

  if (mapsHandler.getPrimitive("singleJointMultipleTips_3", "left_hand_Finger_Spread") != nullptr &&
      mapsHandler.getPrimitive("pinchTight", std::make_pair("left_hand_c", "left_hand_q")) != nullptr ) {
        
    ROSEE::ActionTimed actionTimed ("timed_random");

    actionTimed.insertAction( mapsHandler.getPrimitive("singleJointMultipleTips_3", "left_hand_Finger_Spread"), 0, 0.2, 0, 0.5, "SPREAD");
        
    actionTimed.insertAction( mapsHandler.getPrimitive("pinchTight", std::make_pair("thumb", "pinky")), 0, 0.2, 0, 1, "PINCH");

    actionTimed.print();
        
    yamlWorker.createYamlFile ( &actionTimed, folderForActions + "/timeds/" );
    mapsHandler.parseAllTimeds(folderForActions + "/timeds/");

    std::cout << "The timed action parsed: " << std::endl;
    mapsHandler.getTimed("timed_random")->print();
  }
  

The :code:`insertAction(...)` function will insert the action at the end of the queue. Check the API for further information.









