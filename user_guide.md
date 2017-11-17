# Symbolic nodal analysis User’s Guide
Last update: 10/17/2017

**Introduction:**  This document describes the netlist of the circuit to be analyzed.  A netlist is text file that contains the connectivity information of the circuit.  A netlist in the input to the analysis code and the output are a set of equations that can be solved for the unknown voltages and currents.  The circuits are described in terms of the components and the connections to the other components.  Netlist can be generated with a text editor or exported from a schematic capture program.  LTspice can be used to draw the schematic and a netlist can be exported to the python code to generate the network equations.

**Netlist file format:**  
A circuit description in spice, which is called a netlist, consists of a statements
defining each circuit element and its connection to circuit nodes.  A node is a any point on a circuit where two or more circuit elements meet.  The nodes are numbered from 1 to N in any order and node 0 is the ground node or circuit common.  A ground node is required.  Choose a ground or reference node, which usually is taken to be at a potential of zero volt. All other node voltages constitute n unknowns.  The nodes should be numbered in consecutive order.  Each line in the netlist are either comments, spice directives or circuit elements.  

Spice directives are commands to spice and the first character on the line is a period.  Comment lines start with a * or ;.  The default file extension is ‘.net’.  The python code does some preprocessing of the netlist to check the basic formatting of the netlist is correct.  

The preprocessor performs the following steps:
- remove blank lines and comments
- convert first letter of element name to uppercase
- removes extra spaces between entries
- counts number of entries on each line to make sure the count is correct and counts each element type

The element types that are supported are resistors, capacitors, inductors, independent sources and controlled sources. Each line in the netlist file contains a circuit element.

The format for the element description is  
<letter><name> <n1> <n2> ...[mname] [parvals]  
Where  
<letter> signifies the element type, i.e. R, L, C, V, I, O, E, F, G, H or K 
<name> is a string of letters or numbers that uniquely identify the element
 <...> must be present and [...] is optional.

The element types are described in the following sections.  

**Resistors, capacitors and inductors:**  
The resistors, capacitors and inductors are described by the following line:  
R/L/CXXXXXXX N1 N2 value 
Where:  
XXX = the name of the component, can be any length  
N1 = the first terminal  
N2 = the second terminal  
Value = component value in ohms, farads or henrys.  
For example, a resistor named R1 connected between nodes 1 and 2 with a value of 3000 ohms.  
R1 2 4 3000  
Spice supports other parameters, but these are not allowed in this python implementation.

**Coupled inductors:**  
Coupled inductors are not implemented at this time.
Two coupled inductors are described by the following line.  
KXX LYY LZZ VALUE  
The parameters are:  
LYY = the name of the first coupled inductor  
LZZ = the name of the second coupled inductor  
VALUE = the coefficient of coupling, K, where 0 < K  
The orientation of the inductors is determined by the first node, which is considered to be the
dotted node.

**Independent sources:**  
A voltage source is described by the following line.  
VXX N+ N- VALUE  
The parameters are:  
N+ = the name of the positive terminal  
N- = the name of the negative terminal  
VALUE = the value of the DC voltage  

A current source is described by the following line.  
IXX N+ N- VALUE  
The parameters are:  
N+= the name of the positive terminal, current leaves this terminal (pointy end of the arrow)  
N- = the name of the negative terminal
VALUE = the value of the DC current

**Controlled sources:**  
The voltage-controlled dependent sources are defined using statements of the form  
<letter><name> <nout+> <nout-> <nc+> <nc-> <gain>  
where E is a voltage-controlled voltage source, G is a voltage-controlled current source, the
output voltage is connected between nodes nout+ and nout-, and the control voltage is
measured at node nc+ with respect to node nc-.  
Examples:  
E1 5 1 4 3 10 defines a voltage source that makes node 5 a voltage 10*(v4 − v3)
above the voltage at node 1.
G1 2 1 5 8 50 defines a current source connected between node 2 (the + node)
and node 1 and supplying a current 50 *(v5 − v8).

The current-controlled dependent sources are defined by statements of the form  
<letter><name> <nout+> <nout-> <vcontrol> <gain>   
where F is a current-controlled current source, H is a current-controlled voltage source, and the
output current source is connected between nodes nout+ and nout-, with positive current
flowing through the source from node nout+ to nout-. The control current flows from the
positive node of the source vcontrol through the source and out the negative node.
Examples:  
Fds 11 9 Vsens 1.25 defines a current source connected from node 11 to node 9
that generates a current 1.25 times the current flowing through the source Vsens.
H1 30 20 V5 100 defines a voltage source connected from node 30 to node 20 and
supplying a voltage 100 times the current through the source V5.
It is frequently necessary to add a voltage source with value 0 V to the circuit to sense
the control current for these sources.

The direction of positive controlling current flow is from the positive node, through the source, to the negative node of VNAM. VALUE is the current gain. 

**Op Amps:**   
An opamp component is described by the following line.  
OXX N+ N- Vout
The output of the opamp is a voltage source. Two input terminals are at the same potential. 
Notes: The two ideal op-amp assumptions (no input current, no potential difference at inputs) only hold for circuits with negative feedback.  Because of this, the MNA method as applied here can give erroneous results if negative feedback is not present. Need to work on implementing a better opamp model.  

**Example circuit:**  
Follow the link [here](https://github.com/Tiburonboy/Node-Analysis/blob/master/Example48.pdf) and [here](https://cocalc.com/projects/715a4699-f882-4848-af86-6e5c14f24be2/files/example48.ipynb?session=default) for an example of using the python code with an LTspice schematic.  
