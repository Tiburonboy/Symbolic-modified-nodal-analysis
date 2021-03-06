# Symbolic modified nodal analysis
Last update: 12/6/2017  
See backup [folder](https://github.com/Tiburonboy/Symbolic-modified-nodal-analysis/tree/master/backup) for last update.  

**Abstract:** The python code in this [notebook](https://cocalc.com/projects/715a4699-f882-4848-af86-6e5c14f24be2/files/node%20analysis.ipynb?session=default) will read in a spice like circuit netlist file and formulate a set of network equations in symbolic form using sympy. These equations can then be copied to a different notebook where the node voltages can be numerically solved using sympy or numpy.  Linear resistors, capacitors, inductors, independent sources and controlled sources are supported.

**Introduction:** This node analysis code started as a translation from some C code to generate a nodal admittance matrix that I had written in 1988.  I wrote this code for two reasons.  Free versions of Spice for the PC didn't exist at the time and I wanted to use some of the code from the Numerical Recipes in C [[1]](#ref1) Book.  The original C code worked well and calculated numeric solutions.  I then started writing some C code to generate the matrices with symbolic values and then intended to use LISP to symbolically solve the equations.  I didn’t get too far with this effort.  The LISP code would generate huge symbolic strings with no simplification.  The output was a big pile of trash that was not in the least bit useful or decipherable.

In 2014, I started to use python for my little coding projects and engineering calculations.  There are some nice python libraries for numeric and symbolic calculations (such as numpy and sympy), so I decided to try writing a python script to generate the node equations based on the old C code I had written many years before.  Part way into this project I discovered that there is a new nodal analysis technique being taught today in engineering school called the modified nodal analysis [[2]](#ref2)[[3]](#ref3).  My motivation for reviving this coding project is my continued interest in circuit analysis and synthesis.  

**Description:** The modified nodal analysis provides an algorithmic method for generating systems of independent equations for linear circuit analysis.  Some of my younger colleagues at work were taught this method, but I never heard of it until a short time ago.  These days, I never really analyze a circuit by hand, unless it’s so simple that you can almost do it by inspection.  Most problems that an electrical engineer encounters on the job are complex enough that they use computers to analyze the circuits.  LTspice [[4]](#ref4) is the version of spice that I use, since it’s free and does a good job converging when analyzing switching circuits.

My code started initially by following Erik Cheever's Analysis of Resistive Circuits, reference [[5]](#ref5) MATLAB code, to generate modified nodal equations. I somewhat followed his MATLAB file for resistors, capacitors, opamps and independent sources.  The naming of the matrices follows his convention.  The preprocessor and parser code was converted from my old C code.  The use of pandas for a data frame is new and sympy [[6]](#ref6) is used to do the math and the use of element stamps is from reference [[7]](#ref7).

Inductors are being addressed in the D matrix, which is different than the way Erik Cheever's code works.  Erik's code puts inductors into the G matrix as 1/s/L.  My code puts the inductor contribution into the D matrix.  Coupled inductors also affect the D matrix, so it makes sense to allow the inductors to be in the D matrix rather than the G matrix.

**Network equations:** The network equations are a set of independent equations expressed in this code in matrix form.  There is an equation for each node based on Kirchhoff's current law (KCL) [[8]](#ref8) and an equation for each current unknown.   The current unknowns are the currents from the voltages sources, op amps, voltage controlled voltage sources, current controlled voltage sources, current controlled current sources and inductors.

Equation 1 is the form of  the network equations in matrix form.  

$A\cdot X = Z \tag{1}$

The A matrix describes the connectivity of the resistors, capacitors and G type (VCCS) circuit elements.  The column vector X are the unknown node voltages and unknown currents terms from the voltage sources and inductors.  The column vector Z is made of the known voltages and currents.  The A is formed by four sub matrices, G, B, C and D, which are described below.

$A = \begin{bmatrix}G B\\C D\end{bmatrix}$

The matrix G is formed from the coefficients representing the KCL equations for each node.
The positive diagonal of G$_{k,k}$ are the conductance terms of the resistor and capacitor elements connected to node k.  The off diagonal terms of G$_{k,j}$ are the resistors and capacitor conductances connecting node k to node j.  G type elements (VCCS) have input to the G matrix at the connection and controlling node positions.

The B matrix describes the connectivity of the unknown branch currents.  Independent voltage sources, opamps, H, F and E type elements as well as inductors have inputs to the B matrix.

The C matrix describes the connectivity of the unknown branch currents and is mainly the transpose of B matrix, with the exception of the F type elements (CCCS) and includes the E type value. 

The D matrix describes also connectivity of the unknown currents.  The D matrix is composed of zeros unless there are controlled sources and inductors in the network.

The X vector is comprised of the V and J vectors as shown below.   
$X = \begin{bmatrix}V\\J\end{bmatrix}$  
The V vector contains the node voltages which are the voltage unknowns to be solved for.  The J vector contains the unknown currents from each voltage source.

The Z vector is comprised of the I and Ev vectors as shown below.  
$Z = \begin{bmatrix}I\\Ev\end{bmatrix}$  
The I vector contains the known currents and the Ev vector contains the known voltages.  Ev is used because sympy uses e and E sometimes for the constant 2.71, sometimes called Euler's number [[9]](#ref9).

Putting all the parts together:

$\begin{bmatrix}G B\\C D\end{bmatrix} \cdot \begin{bmatrix}V\\J\end{bmatrix} = \begin{bmatrix}I\\Ev\end{bmatrix}$

**Stamps:** Stamps are templates for modifying the B, C and D matrices and facilitate the construction of the matrices. The stamps used in this implementation of the MNA follow the stamps of reference [[7]](#ref7).  

**Code description:**  The code is divided in the following sections.  
Preprocessor:  The preprocessor reads in the netlist text file and removes comments, extra spaces and blank lines.  The first letter of the element type is capitalized to make subsequent parsing of the file easier.  The number of lines are counted and the number of entries on each line are checked to make sure the count is consistent with the element type.

Parser:  The parser code loads the preprocessed netlist into a data frame.  A report is generated which consists of a count of the element types in the netlist. 

Matrix formulation: Each of the matrices and vectors are generated.  

Circuit equation generation:  The circuit equations are generated in a for loop.  Sympy automatically does some simplification according to its default settings.  Two for loops perform the matrix multiplication on the equation.  
$A\cdot X = Z \tag{2})$

**Code validation:**  The python code was verified by several test circuits and comparing the results to LTspice.  A collection of worked circuits can be found in reference [[7]](#ref7).  See the revision history below for an indication of validation performed so far.  Other test circuits can be found in reference [[10]](#ref10).

As of the the last update to the code the following elements are not fully tested:  
- Opamps
- inductors
- coupled inductors

**Usage:**  The input file is a text file called the net list.  It can generated by using a text editor and following the format listed below or by drawing the schematic and exporting the net list.  LTspice can be used to draw the schematic of the circuit to be analyzed.  The bit mapped image of the schematic can be copied and included in a document.  The net list can be exported into the python code.  The python code will generate the circuit equations in symbolic form.  The [User’s guide](https://github.com/Tiburonboy/Node-Analysis/blob/master/user_guide.md) can at on github.

**Change log:**  The code development change log can be found on github [here](https://github.com/Tiburonboy/Node-Analysis/blob/master/Change%20Log.md).

The backup history can also be found on github.
[Backups](https://github.com/Tiburonboy/Node-Analysis/tree/master/backup)

**Survey of other symbolic circuit analysis code:**  The python code presented in this notebook is somewhat unique since python is open source, free and runs op a variety of platforms, the code presented in this ipython notebook is portable.  As described below, this code is made available under a public domain licence and archived in a github repository.  

There are other symbolic circuit analysis codes available and some of these are described here.  Some of these codes are based on commercial software such as MATLAB [[11]](#ref11), TINA  [[12]](#ref12) and Maple [[13]](#ref13).  

[SLiCAP](https://www.analog-electronics.eu/slicap/slicap.html) is a symbolic linear analysis tool.  SLiCAP runs in MATLAB.  

[TINA](https://www.tina.com) is an acronym of Toolkit for Interactive Network Analysis.  The TINA design suite is a circuit simulator and PCB design software package for analyzing, designing, and real time testing of analog, digital, HDL, MCU, and mixed electronic circuits and their PCB layouts. TINA has some [symbolic analysis capability](https://www.tina.com/symbolic-analysis).

Maple is a mathematical package and there is an application [notes](https://www.maplesoft.com/applications/view.aspx?SID=1427) available describing it use in symbolic circuit analysis. The application note presents an method for evaluating, solving and designing a common, but not so simple pulse-mode high-gain transimpedance amplifier or TIA circuit. 

[Symbolic Circuit Analysis](https://rodanski.net/ben/work/symbolic/index.htm) is a web page devoted to symbolic circuit analysis.  

[SAPWIN](http://www.ewh.ieee.org/soc/es/May2001/12/Begin.htm) is a windows program package for symbolic and numerical simulation of analog circuits.

[Lcapy](https://github.com/mph-/lcapy) is an experimental Python package for teaching linear circuit analysis. It uses SymPy for symbolic mathematics. 

**License:**  This work (includes python code, documentation, test circuits, etc.) is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.  
Share — copy and redistribute the material in any medium or format  
Adapt — remix, transform, and build upon the material for any purpose, even commercially.  
https://creativecommons.org/licenses/by-sa/4.0/  

<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" />

**References:**  
<a id='ref1'></a>
1. Numerical Recipes in C: The Art of Scientific Computing, William H. Press, Brian P. Flannery, Saul A. Teukolsky, William T. Vetterling, Cambridge University Press; 1988
<a id='ref2'></a>
1. The modified nodal approach to network analysis, Chung-Wen Ho, A. Ruehli, P. Brennan, IEEE Transactions on Circuits and Systems ( Volume: 22, Issue: 6, Jun 1975 )
<a id='ref3'></a>
1. [Modified nodal analysis](https://en.wikipedia.org/wiki/Modified_nodal_analysis), wikipedia.org, retrieved October 6, 2017
<a id='ref4'></a>
1. [LTspice](http://www.linear.com/solutions/ltspice), Linear Technology Corporation, retrieved October 6, 2017
<a id='ref5'></a>
1. [Analysis of  Resistive Circuits](http://www.swarthmore.edu/NatSci/echeeve1/Ref/mna/MNA1.html), retrieved October 6, 2017
<a id='ref6'></a>
1. [Sympy](https://www.scipy.org/), Scipy.org, retrieved October 8, 2017
<a id='ref7'></a>
1. ECE 570 Session 3, Computer Aided Engineering for Integrated Circuits, http://www2.engr.arizona.edu/~ece570/session3.pdf
<a id='ref8'></a>
1. [Kirchhoff's circuit laws](https://en.wikipedia.org/wiki/Kirchhoff%27s_circuit_laws), Wikipedia.com, retrieved October 8, 2017
<a id='ref9'></a>
1. [e (mathematical constant)](https://en.wikipedia.org/wiki/E_(mathematical_constant)), Wikipedia.com, retrieved October 8, 2017
<a id='ref10'></a>
1. Solved Problems, A Source of Free Solved Problems,[Category Archives: Electrical Circuits](http://www.solved-problems.com/circuits/electrical-circuits-problems/716/supernode-dependent-voltage-source/), retrieved October 6, 2017
<a id='ref11'></a>
1. [MATLAB](https://www.mathworks.com/products/matlab.html), retrieved October 6, 2017
<a id='ref12'></a>
1. [TINA](https://www.tina.com/), retrieved October 6, 2017
<a id='ref13'></a>
1. [Maple](https://www.maplesoft.com/), retrieved October 6, 2017

