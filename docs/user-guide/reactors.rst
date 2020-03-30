.. _reactors:

Reactors
=========

Conversion Reactor
------------------

The **Conversion Reactor** is used to calculate the mole fraction of components at outlet stream when the conversion of base component for the reaction is defined.

The conversion reactor model have following connection ports:

 * Two Material Streams:

	* feed stream
	* outlet stream

 * One Energy Stream:

 	* heat added

The following features are available for courses

    * **Course Name**
        Click on course name link to view all the enrolled, rejected and requested students list. Moderator can accept or reject the student.
    * **Module Name**
        Click to edit a module added to the course
    * **Lesson or Quiz Name**
        Click to edit a Lesson or Quiz added to the course

To simulate a conversion reactor, following calculation parameters must be provided:

 - Calculation Mode ``CalcMode``
 - Outlet Temperature ``Tdef`` (If calculation mode is Define_Out_Temperature)
 - Number of Reactions ``Nr``
 - Base Component ``BC_r``
 - Stoichiometric Coefficient of Components in Reaction ``Coef_cr``
 - Conversion of Base Component ``X_r``
 - Pressure Drop ``Pdel``

All the above variables are of type parameter Real except the first one (CalcMode) which is of type parameter String. It can have either of the sting values among following:

 - ``Isothermal``: If the reactor is operated isothermally
 - ``Define_Out_Temperature``: If the reactor is operated at specified outlet temperature
 - ``Adiabatic``: If the reactor is operated adiabatically

During simulation, their values can specified directly under ``Reactor Specifications`` by double clicking on the reactor model instance.

Simulating a Conversion Reactor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 1. Create a package named ``CR``

 2. Create a model named ``ms`` inside ``CR``. This is to extend ``MaterialStream`` model

 3. Extend the model ``MaterialStream`` and necessary property method from ``ThermodynamicPackages`` ::

		extends Simulator.Streams.MaterialStream;
		extends Simulator.Files.ThermodynamicPackages.NRTL;

 4. Create another new model named ``conv_react`` inside ``CR``. This is to extend ``ConversionReactor`` model

 5. Extend the model ``ConversionReactor`` from ``UnitOperations`` package ::

		extends Simulator.UnitOperations.ConversionReactor;
  
 6. Extend the ``ConversionReaction`` model from ``ReactionManager`` package available under ``Models`` under ``Files`` package::
  
		extends Simulator.Files.Models.ReactionManager.ConversionReaction;
		
 7. Create another new model named ``test`` inside ``CR``
 
 8. Similar to the ``MaterialStream`` example model, import ``ChemsepDatabase`` and create variables 
	for the compounds which are to be used from ``ChemsepDatabase`` ::
	
		import data = Simulator.Files.ChemsepDatabase;
		parameter data.Ethylacetate etac;
		parameter data.Water wat;
		parameter data.Aceticacid aa;
		parameter data.Ethanol eth;

 9. Define variables for Number of components ``Nc`` and component array ``C``. 
	Also assign the variables created for the compounds to the component array ::
	
		parameter Integer Nc = 4;
		parameter data.GeneralProperties C[Nc] = {etac, wat, aa, eth};
		
 10. Now, create two instances of the ``MaterialStream`` model ``ms`` as we require two material streams which will 
    go as input and comes out as output.
	To do this, open diagram view of ``test`` model, drag & drop ``ms`` for six times for six input streams as shown 
	in fig. Name the instances as ``S1`` and ``S2``
	
 11. Now, create an instance of the ``ConversionReactor`` model ``conv_react``. 
	In the diagram view of ``test`` model, drag & drop ``conv_react``. A pop up will appear asking the name of the component.
	Enter the name as ``B1``
	
 12. 

Plug Flow Reactor
------------------


Simulating a Plug Flow Reactor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~