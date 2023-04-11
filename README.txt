!Effect of geometry precision and load distribution on branch mechanical response
!macros in MECHANICAL APDL (ANSYS PARAMETRIC DESIGN LANGUAGE), ANSYS, Inc. 
!==========================================================================
!To run macros they all needs to be in working directory together with .db files.
!
For both cases of model scan-based and beam define input parameter,file is loaded in each macro:
00. PARAM.MAC
!
!========SCAN-BASED SOLID MODEL======
!Follow the order of macros. Except the interactive one: pick.mac;pick_load.mac >
!the GUI is not needed to save time. 
!For full automatization the change of pick.mac and pick_load.mac is needed. 
!You can combine pick.mac and pick_load.mac to one file,but control of assigned properties after scan.mac
!is recommended.
!
Input files: 
a) param.mac < side_par (parameters of side branches), diam_main (parameters of main branch)

Macros:
01. MESH.MAC - no GUI needed (input: scan.db; output:scanmeshed.db)
 !Meshing of imported geometry 

02. PICK.MAC - GUI needed (input: scanmeshed.db; output:main_cm.db)
 a) Selection of side branches for segmentation process.
    The top of side branches has to  be selected manually, step-by-step. 
 b)Whole side branches has to be selected (unselected) by polygons manually, step-by-step.
   The main axis has to be without side branches to create properly oriented csys.

03. SCAN.MAC - no GUI needed  (input: main_cm.db; output:main_all.db)
 Assignment of material properties and ESYS
 a) First side branches are defined
 b) Second main axis is defined -main axis overwrites previous definitions if they are not unselected (2b).

04. PICK_LOAD.MAC - GUI needed (input: main_all.db; output:main_dof.db)
 a) Select points for load application - in scenario with extracted diameters (6,8) is not needed, but>
 !scan_only parameter needs to be changed; scan_only=1 for manually added load points(pick_load.mac), scan_only=2 for extracted diameters_sc!
 b) Select k-point at the base and in the middle of base part of first side branch.
    Is used for branch positioning and definition of anchorage position. 
 c) Base of branch nearest to the center of gravity.

05. CALC.MAC - no GUI needed (input: main_dof.db; output:scan_final.db)
 a) The branch positioning can be ran in cycles. 
 b) Loading scenarios can be ran in cycles.
 In param.mac decide if you want to use only extracted diameters_sc for scenario 6 and 8.by parameter scan_only.
 And select from which part of side branch do you want to extract diameter: a) from top of branch n_diam=3, b) from 2nd component bellow top n_diam=4.
 Input files: diam_sc - write extracted diameters into table; res_sc - write results
!
!========BEAM MODEL======
06.BEAM.MAC
Input files: 
a) param.mac < side_par, diam_main
b) pos_s - write side branches base position
c) res_b - write results file
Loading scenarios can run in cycles.
