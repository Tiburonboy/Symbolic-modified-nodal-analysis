# Change Log 
Last update: see last entry below for date of last update.

Updates or changes:  
**7/1/2015**  
Ver 1 - coding started, derived from network.c code  
**8/18/2017**  
changed approach, now implementing a modified nodal analysis  
**8/19/2017**  
Wrote some code to generate symbolic matrices, works ok, so heading down the sympy path. Basic debugging finished, but still need to verify some circuits using Ls and Cs.  
**8/30/2017**  
Started to add code for op amps  
**9/1/2017**  
Code added to process op amps  
**9/3/2017**  
Added code to remove spice directives. Fixed orientation of current sources in I matrix. N2 is the arrow end of the current source.  
**9/5/2017**  
After doing some verification testing with inductors and capacitors, it seems that inductors are not being treated correctly.  According to some research, inductor stamp affects the B,C and D arrays.  Erik Cheever's code puts inductors into the G matrix as 1/s/L.  LTspice  results are different than the python code.  Capacitors seem to work OK. Plan is to add controlled sources, then get inductors working.  
**9/6/2017**  
opamp_test_circuit_426 is not working.  Results not the same as LTspice Chebyshev_LPF_1dB_4pole: cut off frequency not correct, other features look OK still need to debug opamps and inductors Adding: VCCS = G type branch element: G needs to be modified CCVS = H type branch element: B, C and D need to be modified  
**9/10/2017**  
researching formulation of B matrix what about a network with only 1 current source?  The B, C and D matrix would be 0 by 0. Think about changing the name of the G matrix to Yr, to keep same as Ho's IEEE paper.  
CCVS = H type branch element: B, C and D need to be modified  
CCCS = F type branch element: B, C and D need to be modified  
VCCS = G type branch element: G needs to be modified  
VCVS = E type branch element: B and C need to be modified  
For CCCS = F type branch elements and CCVS = H type branch elements, need to add a zero volt voltage source to the net list through which the current flows.  It is necessary to add to the net list one extra voltage source for every F and H type element.  
**9/12/2017**  
still working on the B matrix  
**9/18/2017**  
still debugging B matrix, looks like we don't need find_vname() or df2.  This is because a zero volt voltage source is add to the netlist in spice. need to add cccs type to the list of i_unk. Filled out some B matrices by hand and got the same answer as the code.  
**9/30/2017**  
debugging B, C & D matrices  
vcvs is a E type element and sympy didn't like it, fixed problem in parser by changing the lable E to Ea.  Also changed the E matrix name to Ev.  Code seems to run, just need to verify it.  Need to find a new name for func1.  
**10/1/2017**  
Cleaning up comments and notes.  
**10/4/2017**  
Fixed incrementing of sn in D matrix.  D matrix needs to count all i_unks.  In find_vnam() fixed col_num to return just the row number in df2.  
**10/5/2017**  
Fixed E type in matrix C.  Added cccs to i_unk count.  Fixed E type in D.  Fixed J matrix. need to look at the equations next, they don't look correct.  
**10/7/2017**  
Updates to the comments. Verifying equations with hand generated KCL equations.  Fixed H type in D.  
**10/9/2017**  
Still verifying code.  Fixed F type in C.  Still need to verify op amps, inductors and capacitors.
**11/19/2017**
Fixing some documentation.  
**11/30/2017**  
Fixing C matric for op amps, one test case ran OK  
References use in the debugging of the opamp stamp:  
Design of Analog Circuits Through Symbolic Analysis edited by Mourad Fakhfakh, Esteban Tlelo-Cuautle, Francisco V. Fern├índez  
Computer Aided Design and Design Automation edited by Wai-Kai Chen  
http://users.ecs.soton.ac.uk/mz/CctSim/chap1_4.htm  
**12/2/2017**  
Added code for coupled inductors, not tested yet.  
**12/6/2017**  
Testing code for coupled inductors. Changes made to D matrix code. test_circuit_9 seems to give the correct results.  
**18 Feb 2022**  
Noticed a bug when independent voltage source is last line in net list, moving to 1st line in netlist generated what seems like correct equations.  Investigation continues.  For now, ordering the net list with sources first seems to work. Problem is with generation of the Ev and J matrices. In this version, I corrected some grammer and spelling errors.  
**11/14/2023**  
New version of Sympy does not support non-Expr objects in a Matrix. Code to generate the circuit equations towards the end of the notebook produced an error. This line generated the error:  $equ[i] = Eq(eq\_temp,Z[i])$. See the note [here](https://docs.sympy.org/latest/explanation/active-deprecations.html#deprecated-non-expr-in-matrix) for details. In SymPy 1.8 and earlier versions it was possible to put non-Expr elements in a Matrix and the matrix elements could be any arbitrary Python object. Corrections have been made in this notebook. At the time of dubugging this issue, I'm running sympy.__version__  = '1.11.1'  
**29 Nov 2023**  
Problem - When the D matrix is built, independent voltage sources are processed in the data frame order when building the D matrix. If the voltage source followed element L, H, F, K types in the netlist, a row was inserted that put the voltage source in a different row in relation to its position in the Ev matrix. This would cause the node attached to the terminal of the voltage source to be zero volts.
Solution - added code to move voltage source types to the beginning of the net list dataframe before any calculations are performed.  
**1/9/2024**  
Code for building matrix A was not including matrix D if i_unk == 1. The following line of code is new: $A[n,n] = D[0]$ also with a comment. Added verification tests test_14 and test_15 for the case of i_unk == 1 and 0.  

end of log  
