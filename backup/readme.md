Backup History

| filename | description |
|:---------|:------------|
| node_analysis_8_18_2017.ipynb | Starting file, generated from my old c code.  I didn’t complete the translation and I changed the  approach.  I'm now going to implement Modified Nodal Analysis solution. | 
| node analysis_8-19-2017.ipynb | This backup contains a mix of numpy and sympy code.  Probably will go symbolic, so this backup has the numpy code up to that point. | 
| node analysis_9-1-2017.ipynb | This backup has some code clean up.  I added op amps, but I have not debugged the code yet.  I moved to the comments and text to their own mark down cells for ease of reading and to make the notebook look better. |
| node analysis_9-3-2017.ipynb | Added code to remove spice directives.  Fixed orientation of current sources in I matrix.  N2 is the arrow end of the current source. |
| node analysis_9-5-2017a.ipynb | After doing some verification testing with inductors and capacitors, it seems that inductors are not being treated correctly.  According to some research, inductor stamp affects the B,C and D arrays.  Erik Cheever's code puts inductors into the G matrix as 1/s/L.  LTspice results are different than the python code.  Capacitors seem to work OK. |
| node analysis_9-10-2017a.ipynb | Updating the code for the B matrix, saving off work in progress. |
