#### Backup History
A collection of file backups made during the code development.  I usually try to remember to make a backup whenever I get someting working or have made a lot of changes.  Sometimes the backups are just restore points in case I need to back track for some reason. For example the way the inductors were being treated by Eric is different, so this ended up being a point where I made a backup before ditching his code.

| filename | description |
|:---------|:------------|
| node_analysis_8_18_2017.ipynb | Starting file, generated from my old c code.  I didn’t complete the translation and I changed the  approach.  I'm now going to implement Modified Nodal Analysis solution. | 
| node analysis_8-19-2017.ipynb | This backup contains a mix of numpy and sympy code.  Probably will go symbolic, so this backup has the numpy code up to that point. | 
| node analysis_9-1-2017.ipynb | This backup has some code clean up.  I added op amps, but I have not debugged the code yet.  I moved to the comments and text to their own mark down cells for ease of reading and to make the notebook look better. |
| node analysis_9-3-2017.ipynb | Added code to remove spice directives.  Fixed orientation of current sources in I matrix.  N2 is the arrow end of the current source. |
| node analysis_9-5-2017a.ipynb | After doing some verification testing with inductors and capacitors, it seems that inductors are not being treated correctly.  According to some research, inductor stamp affects the B,C and D arrays.  Erik Cheever's code puts inductors into the G matrix as 1/s/L.  LTspice results are different than the python code generated network equations.  Capacitors seem to work OK. |
| node analysis_9-10-2017x.ipynb | Updating the code for the B matrix, saving off work in progress. x = a, b, c, ... |
| node analysis_9-11-2017.ipynb | Saving off work in progress. Created a new data frame for the branches that generate unknown currents.  Still need to give the new functions better names. |
| node analysis_9-12-2017.ipynb | Saving off work in progress. |
| node analysis_9-18-2017.ipynb | Debugging B matrix, looks like we don't need find_vname() or df2.  This is because a zero volt voltage source is add to the net list in spice. Needed to add cccs type to the list of i_unks. Filled out some B matrices by hand and got the same answer as the code. Need to work on the C matrix next. |
| node analysis_9-30-2017ab.ipynb | debugging B, C & D matrices, vcvs is a E type element and sympy didn't like it, fixed problem in parser by changing the lable E to Ea.  Also changed the E maxtrix name to Ev.  Code seems to run, just need to verify it.  Need to find a new name for func1. |
| node analysis_10-1-2017a.ipynb | Cleaning up comments and notes. |
| node analysis_10-4-2017.ipynb | Fixed incrementing of sn in D matrix.  D matrix needs to count all i_unks.  In find_vnam() fixed col_num to return just the row number in df2. |
| node analysis_10-5-2017 & a.ipynb | Fixed E type in matrix C.  Added cccs to i_unk count.  Fixed E type in D. need to look at the equations next, they don't look correct.|
| node analysis_10-8-2017.ipynb | Still verifing code.  Fixed F type in C. Still need to verify op amps, inductors and capacitors. |
| node analysis_11-19-2017.ipynb | Updating documentation |
| node analysis_11-30-2017.ipynb | Fixed C matrix for opamps, ran one test case, worked OK |
| node analysis_12-2-2017a.ipynb | Updated code for coupled inductors, Matrix D code changed, not tested yet |
| node analysis_12-6-2017.ipynb | Fixed matrix D for coupled inductors.  Limited testing |
