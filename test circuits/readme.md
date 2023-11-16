## Validation Test Circuits  
Last updated: 14 Nov 2023

This folder contains the test circuits used to validate the network analysis code.   

Basic validation of the code consisted of analyzing simple networks and examining the results. A more comprehensive evaluation of the code was performed by solving test circuits and comparing the results to LTSpice. As of October 2023 all the element types have been tested. The validation circuits range from simple to large and complex. The largest validation circuit consist of 32 nodes, 59 branches and multiple instances all of the element types. For this large test circuit, there are small numerical differences between the Python MNA code results and the LTSpice solution, which are describe in the JupyterLab notebooks.

The table below lists the test circuits used to validate and test the code. Each test circuit has it’s own folder with the support files. The test circuits are a collection of problems from text books or large and complex circuits that I made up that include various elements or unique combinations of elements or topologies. In LTSpice the series resistance for inductors defaults to 1e-3 Ohms unless set to zero. Leaving the default series inductance at 1e-3 Ohms will cause small numerical differences in the solution obtained by LTSpice.

| Test file | Circuit Description | notes |
|:---------------|-------------|--------|
| test_1.asc | Resistors, independent and dependent sources | DC analysis |
| test_2.asc | Resistors and independent sources | DC analysis |
| test_3.asc | Same circuit as test_1, but with R2 replaced with C2 and R3 replaced with L1. | AC analysis |
| test_4.asc | Resistors and one of each type of source | AC analysis |
| test_5.asc | RLC and one of each type of source | AC analysis |
| test_6.asc | RLC, coupled inductors and one of each type of source | AC analysis |
| test_7.asc | Large circuit and two or more of each element type w/o Op Amps | AC analysis |
| test_X | I have 13 test circuits, write ups still in progress | NA |

The following file types are in each folder:

| file name | Description |
|:---------------|-------------|
| test_*.asc | LTSpice circuit schematic |
| readme.md | a description of the circuit |
| test_*.ipynb | test report Jupyter notebook |
| test_*.net | Spice net list |	
| test_*.png | image of the schematic for display in the notebook |
| test_*.csv | csv data exported from LTSpice |

