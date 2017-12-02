# Change Log
Symbolic modified nodal analysis  
Last update: see last entry below for date of last update.

Date started: April 17, 2017  
file name: node analysis.ipynb  
Requires: Python version 3 or higher and a jupyter notebook  
Author: Tony  

Updates or changes:  
7/1/2015  
Ver 1 - coding started, derived from network.c code  
8/18/2017  
changed approach, now implementing a modified nodal analysis  
8/19/2017  
Wrote some code to generate symbolic matrices, works ok, so heading down the sympy path. Basic debugging finished, but still need to verify some circuits using Ls and Cs.  
8/30/2017  
Started to add code for op amps  
9/1/2017  
Code added to process op amps  
9/3/2017  
Added code to remove spice directives. Fixed orientation of current sources in I matrix. N2 is the arrow end of the current source.  
9/5/2017  
After doing some verification testing with inductors and capacitors, it seems that inductors are not being treated correctly.  According to some research, inductor stamp affects the B,C and D arrays.  Erik Cheever's code puts inductors into the G matrix as 1/s/L.  LTspice  results are different than the python code.  Capacitors seem to work OK. Plan is to add controlled sources, then get inductors working.  
9/6/2017  
opamp_test_circuit_426 is not working.  Results not the same as LTspice Chebyshev_LPF_1dB_4pole: cut off frequency not correct, other features look OK still need to debug opamps and inductors Adding: VCCS = G type branch element: G needs to be modified CCVS = H type branch element: B, C and D need to be modified  
9/10/2017  
researching formulation of B matrix what about a network with only 1 current source?  The B, C and D matrix would be 0 by 0. Think about changing the name of the G matrix to Yr, to keep same as Ho's IEEE paper.  
CCVS = H type branch element: B, C and D need to be modified  
CCCS = F type branch element: B, C and D need to be modified  
VCCS = G type branch element: G needs to be modified  
VCVS = E type branch element: B and C need to be modified  
For CCCS = F type branch elements and CCVS = H type branch elements, need to add a zero volt voltage source to the net list through which the current flows.  It is necessary to add to the net list one extra voltage source for every F and H type element.  
9/12/2017  
still working on the B matrix  
9/18/2017  
still debugging B matrix, looks like we don't need find_vname() or df2.  This is because a zero volt voltage source is add to the netlist in spice. need to add cccs type to the list of i_unk. Filled out some B matrices by hand and got the same answer as the code.  
9/30/2017  
debugging B, C & D matrices  
vcvs is a E type element and sympy didn't like it, fixed problem in parser by changing the lable E to Ea.  Also changed the E matrix name to Ev.  Code seems to run, just need to verify it.  Need to find a new name for func1.  
10/1/2017  
Cleaning up comments and notes.  
10/4/2017  
Fixed incrementing of sn in D matrix.  D matrix needs to count all i_unks.  In find_vnam() fixed col_num to return just the row number in df2.  
10/5/2017  
Fixed E type in matrix C.  Added cccs to i_unk count.  Fixed E type in D.  Fixed J matrix. need to look at the equations next, they don't look correct.  
10/7/2017  
Updates to the comments. Verifying equations with hand generated KCL equations.  Fixed H type in D.  
10/9/2017  
Still verifying code.  Fixed F type in C.  Still need to verify op amps, inductors and capacitors.
11/19/2017
Fixing some documentation.  
11/30/2017  
Fixing C matric for op amps, one test case ran OK  
References use in the debugging of the opamp stamp:  
Design of Analog Circuits Through Symbolic Analysis edited by Mourad Fakhfakh, Esteban Tlelo-Cuautle, Francisco V. Fernández  
Computer Aided Design and Design Automation edited by Wai-Kai Chen  
http://users.ecs.soton.ac.uk/mz/CctSim/chap1_4.htm
12/2/2017  
Added code for coupled inductors, not tested yet.  
end of list
