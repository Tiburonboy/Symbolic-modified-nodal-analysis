# Node-Analysis
This notebook contains some python code to read a netlist formatted similar to a SPICE file and generate the node equations.  Nodal analysis is described [here](https://en.wikipedia.org/wiki/Modified_nodal_analysis).  Follows Erik Cheever's Analysis of  Resistive Circuits [page](http://www.swarthmore.edu/NatSci/echeeve1/Ref/mna/MNA1.html) to generate modified nodal equations.  I somewhat followed his matlab file.  

## The following elements are supported:

Resistors: 
RXX N1 N2 VALUE
N1 and N2 are the two element nodes. VALUE is the resistance (in ohms) and may be positive or negative but not zero.

Capacitors
CXX N+ N- VALUE
N+ and N- are the positive and negative element nodes, respectively. VALUE is the capacitance in Farads.

Inductors
LXX N+ N- VALUE
N+ and N- are the positive and negative element nodes, respectively. VALUE is the inductance in Henries.

Independent Sources
VXX N+ N- VALUE
IXX N+ N- VALUE

### Not yet implemented:

Linear Voltage-Controlled Current Sources
GXX N+ N- NC+ NC- VALUE
VALUE is the transconductance (in mhos).

Voltage-Controlled Voltage Sources
EXX N+ N- NC+ NC- VALUE
VALUE is the voltage gain.

Linear Current-Controlled Current Sources
FXX N+ N- VNAM VALUE
VNAM is the name of a voltage source through which the controlling current flows. The direction of positive controlling current flow is from the positive node, through the source, to the negative node of VNAM. VALUE is the current gain.

Linear Current-Controlled Voltage Sources
HXX N+ N- VNAM VALUE
VNAM is the name of a voltage source through which the controlling current flows. 
VALUE is the transresistance (in ohms).

Coupled (Mutual) Inductors (not currently supported)
KXX LYY LZZ VALUE
LYY and LZZ are the names of the two coupled inductors.
VALUE is the coefficient of coupling, K, which must be in the range −1 ≤ K ≤ +1. Using the 'dot' convention, place a 'dot' on the first node of each inductor.

Negative impedance converter
From Wikipedia, the free encyclopedia
The negative impedance converter (NIC) is a one-port op-amp circuit acting as a negative load which injects energy into circuits in contrast to an ordinary load that consumes energy from them. This is achieved by adding or subtracting excessive varying voltage in series to the voltage drop across an equivalent positive impedance. This reverses the voltage polarity or the current direction of the port and introduces a phase shift of 180° (inversion) between the voltage and the current for any signal generator. The two versions obtained are accordingly a negative impedance converter with voltage inversion (VNIC) and a negative impedance converter with current inversion (INIC). The basic circuit of an INIC and its analysis is shown below.

Gyrator
From Wikipedia, the free encyclopedia
A gyrator is a passive, linear, lossless, two-port electrical network element proposed in 1948 by Bernard D. H. Tellegen as a hypothetical fifth linear element after the resistor, capacitor, inductor and ideal transformer.[1] Unlike the four conventional elements, the gyrator is non-reciprocal. Gyrators permit network realizations of two-(or-more)-port devices which cannot be realized with just the conventional four elements. In particular, gyrators make possible network realizations of isolators and circulators.[2] Gyrators do not however change the range of one-port devices that can be realized. Although the gyrator was conceived as a fifth linear element, its adoption makes both the ideal transformer and either the capacitor or inductor redundant. Thus the number of necessary linear elements is in fact reduced to three. Circuits that function as gyrators can be built with transistors and op amps using feedback.

OpAmp

