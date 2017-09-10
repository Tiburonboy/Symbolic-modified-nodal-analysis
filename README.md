# Node-Analysis
This project is about an ipython notebook to generate network equations.

**Abstract:** This notebook will read in a spice like circuit netlist file and compute the network equations. These equations can then be copied to a different notebook where the node voltages can be solved using the power of sympy or numpy.

**Description:** This node analysis code started as a translation from some C code to generate a nodal admittance matrix that I had written in 1988.  The original C code worked well and calculated numeric solutions.  I then started writing some C code to generate the matrices with symbolic values and then intended to use LISP to symbolically solve the equations.  I didn’t get too far with this effort.  The LISP code would generate huge symbolic strings with no simplification.  The output was a big pile of trash that was not in the least bit useful or decipherable.  

In 2014, I started to use python for my little coding projects and engineering calculations.  There are some nice python libraries for numeric and symbolic calculations (such as numpy and sympy), so I decided to try writing a python script to generate the node equations based on the old C code I had written many years before.  Part way into this project I discovered that there is a new nodal analysis technique being taught today in engineering school called the modified nodal analysis (1,2).  The modified nodal analysis provides an algorithmic method for generating systems of independent equations for linear circuit analysis.  Some of my younger colleagues at work were taught this method, but I never heard of it until a short time ago.  These days, I never really analyze a circuit by hand, unless it’s so simple that you can almost do it by inspection.  Most problems that an electrical engineer encounters on the job are complex enough that they use computers to analyze the circuits.  LTspice is the version of spice that I use, since it’s free and does a good job converging when analyzing switching circuits.  

The code follows Erik Cheever's Analysis of  Resistive Circuits [page](http://www.swarthmore.edu/NatSci/echeeve1/Ref/mna/MNA1.html) to generate modified nodal equations. I somewhat followed his matlab file for resistors, capacitors, opamps and independent sources.  The preprocessor and parser code was converted from my old C code.  The use of pandas for a data frame is new and sympy is used to do the math.

After doing some verification testing with inductors and capacitors, it seems that inductors are not being treated correctly.  According to some research, inductor stamp affects the B,C and D arrays.  Erik Cheever's code puts inductors into the G matrix as 1/s/L.  LTspice results are different than the python code.  Capacitors seem to work OK.

References:
1. The modified nodal approach to network analysis, Chung-Wen Ho, A. Ruehli, P. Brennan, IEEE Transactions on Circuits and Systems ( Volume: 22, Issue: 6, Jun 1975 )
2. https://en.wikipedia.org/wiki/Modified_nodal_analysis

