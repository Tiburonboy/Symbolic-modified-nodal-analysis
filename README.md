# Symbolic modified nodal analysis
Last update: 10/8/2017

**Abstract:** The python code in this notebook will read in a spice like circuit netlist file and formulate a set of network equations in symbolic form using sympy. These equations can then be copied to a different notebook where the node voltages can be numerically solved using sympy or numpy.  Linear resistors, capacitors, inductors, independent sources and controlled sources are supported.

**Introduction:** This node analysis code started as a translation from some C code to generate a nodal admittance matrix that I had written in 1988.  I wrote this code for two reasons.  Free versions of Spice for the PC didn't exist at the time and I wanted to use some of the code from the Numerical Recipes in C [[1]](#ref1) Book.  The original C code worked well and calculated numeric solutions.  I then started writing some C code to generate the matrices with symbolic values and then intended to use LISP to symbolically solve the equations.  I didn’t get too far with this effort.  The LISP code would generate huge symbolic strings with no simplification.  The output was a big pile of trash that was not in the least bit useful or decipherable.

In 2014, I started to use python for my little coding projects and engineering calculations.  There are some nice python libraries for numeric and symbolic calculations (such as numpy and sympy), so I decided to try writing a python script to generate the node equations based on the old C code I had written many years before.  Part way into this project I discovered that there is a new nodal analysis technique being taught today in engineering school called the modified nodal analysis [[2]](#ref2)[[3]](#ref3).  My motivation for reviving this coding project is my continued interest in circuit analysis and synthesis.  

**Description:** The modified nodal analysis provides an algorithmic method for generating systems of independent equations for linear circuit analysis.  Some of my younger colleagues at work were taught this method, but I never heard of it until a short time ago.  These days, I never really analyze a circuit by hand, unless it’s so simple that you can almost do it by inspection.  Most problems that an electrical engineer encounters on the job are complex enough that they use computers to analyze the circuits.  LTspice [[4]](#ref4) is the version of spice that I use, since it’s free and does a good job converging when analyzing switching circuits.

My code started initially by following Erik Cheever's Analysis of Resistive Circuits, reference [[5]](#ref5) MATLAB code, to generate modified nodal equations. I somewhat followed his MATLAB file for resistors, capacitors, opamps and independent sources.  The naming of the matrices follows his convention.  The preprocessor and parser code was converted from my old C code.  The use of pandas for a data frame is new and sympy is used to do the math and the use of element stamps is from reference [[6]](#ref6).

After doing some verification testing with inductors and capacitors, it seems that inductors are not being treated correctly.  I did some research and the inductor stamp affects the B,C and D arrays.  Erik Cheever's code puts inductors into the G matrix as 1/s/L.  LTspice results are different than the python code when analyzing circuits with inductors.  Capacitors seem to work OK.

**Network equations:** The network equations are a set of independent equations expressed in this code in matrix form.  There is an equation for each node based on Kirchhoff's current law (KCL) [[6]](#ref6) and an equation for each current unknown.   The current unknowns are the currents from the voltages sources, op amps, voltage controlled voltage sources, current controlled voltage sources, current controlled current sources and inductors.

Equation 1 is the form of  the network equations in matrix form.  

$A\cdot X = Z \ref(1)$

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
The I vector contains the known currents and the Ev vector contains the known voltages.  Ev is used because sympy uses e and E sometimes for the constant 2.71, sometimes called Euler's number [[6]](#ref6).

Putting all the parts together:

$\begin{bmatrix}G B\\C D\end{bmatrix} \cdot \begin{bmatrix}V\\J\end{bmatrix} = \begin{bmatrix}I\\Ev\end{bmatrix}$

**Stamps:** Stamps are templates for modifying the B, C and D matrices and facilitate the construction of the matrices. The stamps used in this implementation of the MNA follow the stamps of reference [[6]](#ref6).  

**Code description:**  The code is divided in the following sections.  
Preprocessor:  The preprocessor reads in the netlist text file and removes comments, extra spaces and blank lines.  The first letter of the element type is capitalized to make subsequent parsing of the file easier.  The number of lines are counted and the number of entries on each line are checked to make sure the count is consistent with the element type.

Parser:  The parser code loads the preprocessed netlist into a data frame.  A report is generated which consists of a count of the element types in the netlist. 

Matrix formulation: Each of the matrices and vectors are generated.  

Circuit equation generation:  The circuit equations are generated in a for loop.  Sympy automatically does some simplification according to its default settings.  Two for loops perform the matrix multiplication on the equation.  
$A\cdot X = Z \ref(1)$

**Code validation:**  The python code was verified by several test circuits and comparing the results to LTspice.  A collection of worked circuits can be found in reference [[7]](#ref7).  See the revision history below for an indication of validation performed so far.  Other test circuits can be found in reference [[7]](#ref7).

As of the the last update to the code the following features are not fully implemented or tested:
- Opamps
- inductors
- coupled inductors

**Usage:**  The input file is a text file called the net list.  It can generated by using a text editor and following the format listed below or by drawing the schematic and exporting the net list.  LTspice can be used to draw the schematic of the circuit to be analyzed.  The bit mapped image of the schematic can be copied and included in a document.  The net list can be exported into the python code.  The python code will generate the circuit equations in symbolic form.  The user’s guide can be found [here]().

Add a link to github for the user's guide.

[User’s guide](https://github.com/Tiburonboy/Node-Analysis/blob/master/user_guide.md)

**Change log:**  The code development change log can be found on github [here](https://github.com/Tiburonboy/Node-Analysis/blob/master/Change%20Log.md).
 
[Revision history](https://github.com/Tiburonboy/Node-Analysis/blob/master/Rev%20History.md)

**License:**  This work (includes python code, documentation, test circuits, etc.) is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.  
Share — copy and redistribute the material in any medium or format  
Adapt — remix, transform, and build upon the material for any purpose, even commercially.  
https://creativecommons.org/licenses/by-sa/4.0/  

<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" />
