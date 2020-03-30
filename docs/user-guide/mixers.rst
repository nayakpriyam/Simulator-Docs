.. _mixers:

Mixers
=======


Mixer
------

The **Mixer** is used to mix up to any number of material streams into one, 
while executing all the mass and energy balances.

The only calculation parameter for mixer is the outlet pressure calculation mode (outPress) 
variable which is of type parameter String. It can have either of the string values among the 
following modes:
	
 - ``Inlet_Minimum``  Outlet pressure is taken as minimum of all inlet streams pressure
 - ``Inlet_Average``  Outlet pressure is calculated as average of all inlet streams pressure
 - ``Inlet_Maximum``  Outlet pressure is taken as maximum of all inlet streams pressure
	
``outPress`` has been declared of type parameter String. 
During simulation, it can specified directly under Mixer Specifications
by double clicking on the mixer model instance.


Simulating a Mixer
~~~~~~~~~~~~~~~~~~~

 1. Create a package named ``Mixer``
 
 2. Create a model named ``ms`` inside ``Mixer``. This is to extend ``MaterialStream`` model
 
 3. Extend the model ``MaterialStream`` and necessary property method from ``ThermodynamicPackages`` ::
 
	 extends Simulator.Streams.MaterialStreams;
	 extends Simulator.Files.Thermodynamic_Packages.Raoults_Law;
	 

 4. Create another new model named ``mix``
  
 5. Similar to the ``MaterialStream`` example model, import ``ChemsepDatabase`` and create variables 
    for the compounds which are to be used from ``ChemsepDatabase`` ::
	 
	 import data = Simulator.Files.Chemsep_Database;
	 parameter data.Ethanol eth;
	 parameter data.Methanol meth;
	 parameter data.Water wat;
	 
 6. Define variables for Number of components ``Nc`` and component array ``C``. 
    Also assign the variables created for the compounds to the component array ::
	 
     parameter Integer Nc = 3;
     parameter data.General_Properties C[Nc] = {meth, eth, wat};
    
 7. Now, create six instances of the ``MaterialStream`` model ``ms`` as we require six material streams which will 
    go as input. To do this, open diagram view of ``main`` model, drag & drop ``ms`` for six times for six input streams as shown in fig. Name the instances as ``ms1``, ``ms2``, ``ms3``, ``ms4``, ``ms5`` and ``ms6``
	
	.. image:: ../img/mixer-ms-in-drop.png
	
 8. Similarly, create amother instance of the ``MaterialStream`` model ``ms`` which will act as the outlet stream. Name the instance as ``out1``
 
 	.. image:: ../img/mixer-ms-out-drop.png
 
 9. Now, Drag and drop the ``Mixer`` model available under ``UnitOperations``. Name the instance as ``mixer1``
 
 	.. image:: ../img/mixer-drop.png

 10. Now double click on ``ms1``. Component Parameters window opens. Go to Stream Specifications tab. There are two parameter ``Nc`` and ``C`` for which the values are to be entered. 
     As the value for ``Nc`` and ``C`` are already declared earlier in step 6 while defining the variables, these variables are passed here instead of the values. Repeat this for remaining five input material streams and the output material stream.
	 
	  	.. image:: ../img/mixer-in-par.png
	  
 11. Now double click on ``mixer1``. Component Parameters window opens. Go to Mixer Specifications tab and enter the values for parameters as mentioned below:
     
	 - ``Nc`` and ``C`` can be entered same as material stream 
	 - ``NI`` represents the number of input material streams. As there are six material streams going as input, enter 6 against ``NI``
	 - ``outPress`` represents the pressure calculation mode for outlet material stream. Currently mixer support three different 
	   calculation mode which are inlet minimum,inlet average and inlet maximum. Here inlet average will be selected. So enter ``"Inlet_Average"``
	   
	   .. image:: ../img/mixer-par.png
	 
 12. Switch to text view. Following lines of code will be autogenrated ::
	 
	  ms ms1(Nc = Nc, C = C) annotation( ...);
	  ms ms2(Nc = Nc, C = C) annotation( ...);
	  ms ms3(Nc = Nc, C = C) annotation( ...);
	  ms ms4(Nc = Nc, C = C) annotation( ...);
	  ms ms5(Nc = Nc, C = C) annotation( ...);
	  ms ms6(Nc = Nc, C = C) annotation( ...);
	  Simulator.Unit_Operations.Mixer mixer1(Nc = Nc, NI = 6, C = C, outPress = "Inlet_Average") annotation( ...);
	  ms out1(Nc = Nc, C = C) annotation( ...);
  
 13. Now, connect the streams with unit operations. For this, switch back to Diagram view.
 
     .. image:: ../img/mixer-connected.png
 
 14. Switch to text view. Following lines of code will be autogenrated under ``equation`` section :: 
  
		connect(mixer1.outlet, out1.In) annotation( ...);
		connect(ms6.Out, mixer1.inlet[6]) annotation( ...);
		connect(ms5.Out, mixer1.inlet[5]) annotation( ...);
		connect(ms4.Out, mixer1.inlet[4]) annotation( ...);
		connect(ms3.Out, mixer1.inlet[3]) annotation( ...);
		connect(ms2.Out, mixer1.inlet[2]) annotation( ...);
		connect(ms1.Out, mixer1.inlet[1]) annotation( ...);

 15. Specify the value of pressure for all the six inlet material streams ::

	  ms1.P = 101325;
	  ms2.P = 202650;
	  ms3.P = 126523;
	  ms4.P = 215365;
	  ms5.P = 152365;
	  ms6.P = 152568;
    
 16. Specify the value of temperature for all the six inlet material streams ::
 
	  ms1.T = 353;
	  ms2.T = 353;
	  ms3.T = 353;
	  ms4.T = 353;
	  ms5.T = 353;
	  ms6.T = 353;
    
 17. Specify the value of molar flow rate for all the six inlet material streams ::
 
	  ms1.F_p[1] = 100;
	  ms2.F_p[1] = 100;
	  ms3.F_p[1] = 300;
	  ms4.F_p[1] = 500;
	  ms5.F_p[1] = 400;
	  ms6.F_p[1] = 200;
	  
 18. Specify the mole fraction of components for all the six inlet material streams ::
 
	  ms1.x_pc[1, :] = {0.25, 0.25, 0.5};
	  ms2.x_pc[1, :] = {0, 0, 1};
	  ms3.x_pc[1, :] = {0.3, 0.3, 0.4};
	  ms4.x_pc[1, :] = {0.25, 0.25, 0.5};
	  ms5.x_pc[1, :] = {0.2, 0.4, 0.4};
	  ms6.x_pc[1, :] = {0, 1, 0};

 19. This completes the Mixer package. Now click on ``Simulate`` button to simulate the ``mix`` model. Alternatively, you can also
find this package named ``Mixer`` in the ``Simulator`` library under ``Examples`` package.



Splitter
---------

The Splitter is used to split up to a material streams into two, while executing all the 
mass and energy balances.


The only calculation parameter for splitter is the calculation type ``CalcType`` variable 
which is of type parameter String. It can have either of the string values among the following types:
 - ``Split_Ratio`` Mass and molar flow rate of the outlet streams are to be calculated depending on the specified split ratio
 - ``Mass_Flow`` Molar flow rate of the outlet streams are to be calculated depending on the specified mass flow rates of outlet stream
 - ``Molar_Flow`` Mass flow rate of the outlet streams are to be calculated depending on the specified molar flow rate of the outlet stream

``CalcType`` has been declared of type parameter String. 
During simulation, it can specified directly under Splitter Specifications by double clicking 
on the splitter model instance.


Depending on the CalcType specified in the Splitter Specification, its value has to be specified 
through the variable Specification Value ``SpecVal_s``. It is declared of type Real.
During simulation, value of this variable need to be defined in the equation section.


Simulating a Splitter
~~~~~~~~~~~~~~~~~~~~~~

 1. Create a package named ``Splitter``
 
 2. Create a model named ``ms`` inside ``Splitter``. This is to extend ``MaterialStream`` model
 
 3. Extend the model ``MaterialStream`` and necessary property method from ``ThermodynamicPackages`` ::
 
	 extends Simulator.Streams.MaterialStreams;
	 extends Simulator.Files.Thermodynamic_Packages.Raoults_Law;
	 

 4. Create another new model named ``main``
  
 5. Similar to the ``MaterialStream`` example model, import ``ChemsepDatabase`` and create variables 
    for the compounds which are to be used from ``ChemsepDatabase`` ::
	 
	 import data = Simulator.Files.Chemsep_Database;
	 parameter data.Benzene benz;
	 parameter data.Toluene tol;
	 
 6. Define variables for Number of components ``Nc`` and component array ``C``. 
    Also assign the variables created for the compounds to the component array ::
	 
     parameter Integer Nc = 2;
     parameter data.General_Properties C[Nc] = {benz, tol};
    
 7. Now, create three instances of the ``MaterialStream`` model ``ms`` as we require one material stream which will 
    go as input and two material streams which will come as output. To do this, open diagram view of ``main`` model, drag & drop ``ms`` for three times as shown in fig. Name the instances as ``S1``, ``S2`` and ``S3``
	
		.. image:: ../img/splitter-ms-drop.png


 8.  Now, Drag and drop the ``Splitter`` model available under ``UnitOperations``. Name the instance as ``B1``
 
 	    .. image:: ../img/splitter-drop.png

 9. Now double click on ``ms1``. Component Parameters window opens. Go to Stream Specifications tab. There are two parameter ``Nc`` and ``C`` for which the values are to be entered. As the value for ``Nc`` and ``C`` are already declared earlier in step 6 while defining the variables, these variables are passed here instead of the values. Repeat this for remaining two material streams.
	 
	  	.. image:: ../img/splitter-in-par.png
	  
 10. Now double click on ``B1``. Component Parameters window opens. Go to Splitter Specifications tab and enter the values for parameters as mentioned below:
     
	 - ``Nc`` and ``C`` can be entered same as material stream 
	 - ``No`` represents the number of output material streams. As we have two material streams coming out, enter 2 against ``No``
	 - ``CalcType`` represents the calculation type specification for outlet material stream. Currently splitter support three different 
	   calculation type which are split ratio,mass flow and molar flow. Here molar flow will be selected. 
	   So enter ``"Molar_Flow"``
	   
	   .. image:: ../img/splitter-par.png
	 
 11. Switch to text view. Following lines of code will be autogenrated ::
	 
	  ms S1(Nc = Nc, C = C) annotation( ...);
	  ms S2(Nc = Nc, C = C) annotation( ...);
	  ms S3(Nc = Nc, C = C) annotation( ...);
	  Simulator.Unit_Operations.Splitter B1(Nc = Nc, No = 6, C = C, CalcType = "Molar_Flow") annotation( ...);
  
 12. Now, connect the streams with unit operations. For this, switch back to Diagram view.
 
     .. image:: ../img/splitter-connected.png
 
 13. Switch to text view. Following lines of code will be autogenrated under ``equation`` section :: 
  
		connect(B1.Out[2], S3.In) annotation( ...);
		connect(B1.Out[1], S2.In) annotation( ...);
		connect(S1.Out, B1.In) annotation( ...);

 14. Specify the pressure, temperature, component mole fractions and molar flow rate for the inlet material stream ::

	  S1.P = 101325;
  	  S1.T = 300;
  	  S1.x_pc[1, :] = {0.5, 0.5};
  	  S1.F_p[1] = 100;

 15. Now specify the specification value for the selected calculation type in splitter ::

 	  B1.SpecVal_s = {20, 80};
    
 16. This completes the Splitter package. Now click on ``Simulate`` button to simulate the ``main`` model. Alternatively, you can also
 find this package named ``Splitter`` in the ``Simulator`` library under ``Examples`` package.