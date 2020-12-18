# Introduction to GEODYN

A GEODYN run is based on the user inputs that get inputted into the run as a CARD.  The full set of CARDS is referred to as the deck. The deck is located at ```RUNS>INPUTS>iisset_start``` within our file structure. Information on each of the CARDS is in the Volume 3 Documentation.

## AWS Directory Structure:

```
IIE  
    ORIG/
        (thousands of subroutines)
    MODS/
        (any modifications to the above subroutines)
        
IIS  
    ORIG/
        (thousands of subroutines)
    MODS/
        (any modifications to the above subroutines)
RUNS  
    (output and input files for GEODYN)
```




### IIS
Description (Listed from Vol 5 page 5)
 - GEODYN IIS program reads and interprets the option cards  
 - Reads the input observation data  
 - Read the optional gravity model, station geodetics, and area/mass files  
 - Extracts ephemeris data from necessary files and tables given the input time periods
 - Rearranges the data into vector form to minimize the amount of data manipulation in IIE
 - Output is put into 2 files  
     - (Fort 11) One contains the data from IIS, and the other, (Fort 41) contains all the information to run IIE (i.e. user selection, appropriate ephemeris, flux, polar motion and time data; the pointers and the sizes for the dynamic arrays; the defaults for all model parameters; and all control information needed to output the requested files.
        

### IIE
Description (Listed from Vol 5 page 5)  
 - IIE performs all the computations normally associated with satellite orbit and geodetic parameter estimation programs.  
 - IIE is written to run efficiently on vector processing computers without having to handle the I/O intensive parts that are performed by IIS.

#### Optional Outputs from IIE  

Built-in File (binary) Outputs:
- Trajectory File (``ORBFIL``)
- Residual File (``RESIDU``)
- Partial Derivative File
- Normal Equation File
- Force Model Partial Derivative File

Optional Printouts:  

- Residual Printouts to iieout   (``OBSVU``   -- Vol 3, sec 2.3.49)
- Trajectory Printouts to ascii files (``ORBTVU`` -- Vol 3, sec 2.5.29)

#### Card Inputs that Control Residual and Trajectory Outputs  

**OBSVU**  
Controls residual printout for all arcs.
- = 4 Indicates residuals are requested on all iterations for all arcs .
- = 5 Indicates no residuals are requested for any arc.

**RESIDU**  
Requests residual file output on unit 19 on the last inner of the last global iteration .
- Column 7  
    - 0 - Residual file will not contain event times or trajectory information .  
    - 1 - Requests event times and spacecraft trajectory or true pole station locations at those event times be output along with the residual information . Trajectory output is in the true of a data coordinate system.
- Column 8
    - 0 - Residual file will not contain observation data .
    - 1 - Requests observation data be output along with the residual information .
    - 2 - Requests observation data and observation correction data be output along with the residual information.


**ORBFIL**  
Requests output of trajectory file (s) on specified unit (s) on the last iteration of the run.
- Column 7 -- Coordinate system of output
    - 0 - True of date ( default ) 0
    -1 - True of reference date
    -2 - Mean of year 2000
- Column 8 -- Switch indicating whether trajectory file
    - 0 - Single satellite
    - 1 - Set of satellites . This option has meaning only when used in conjunction with sets of satellites (See EPOCH and SLAVE option cards for more details ). If satellite ID in columns 18 -24 is a master satellite , then the trajectory for all satellites in the set will be output .
- Columns 9 -11 -- Mandatory unit number for trajectory file . All trajectory files within an arc must have unique unit numbers . The suggested unit number starts at 130. [3]
    - This one is very confusing
    - 31 was listed before ??
- Columns 18-24 -- Satellite ID
    - ISS -- 9806701
- Columns 25-44 -- Start date and time for trajectory output ( YYMMDDHHMMSS .SS ) (NO DEFAULT)
    - ISS start: 190420211800.0
- Columns 45-59 -- Stop date and time for trajectory output ( YYMMDDHHMMSS .SS ).
    - ISS stop: 190422 24200.00
- Columns 60-72-- Time interval between successive trajectory outputs (seconds)
    - ISS .100000D+01
    

**ORBTVU**  
Requests trajectory printout . Cartesian elements on unit 8 and Keplerian elements on unit 10.

- Column 7- Frequency of trajectory output
    - 0 - Trajectory output viewed between times specified in columns 25 -59 and at interval specified in columns 60 -72.
    - 1 - Trajectory output viewed between times specified in columns 25 -59 at data points only .
    - 2 - Trajectory output viewed between times specified in columns 25 -59 at data points and at the interval specified in columns 60 -72.
- Column 8 - Coordinate system of output
    - 0 - True of date 0
    - 1 - True of reference date
    - 2 - Mean of year 2000
- Column 9 - Trajectory type indicator .
- Column 10 - Iterations on which trajectory will be printed .
    - 0 - First arc iter of first global iter
    - 1 - Last arc iter of last global iter
    - 2 - Both first first and last last
    - 3 - All iterations
- Columns 18 - 24 --  Satellite ID. If not specified, applies to all spacecraft  in arc .
- Columns 25 - 44  --   Start date and time for trajectory viewing ( YYMMDDHHMMSS .SS ).
- Columns 45 - 59 --  Stop date and time for trajectory viewing ( YYMMDDHHMMSS .SS ).
- Columns 60 - 72  --   Nominal interval between successive trajectory viewings .


## RUNS
The RUNS directory contains all of the relevant information for running GEODYN.  The main script that generates a GEODYN run is `zzz`.
This directory contains all of the data inputs and outputs from GEODYN.


### Including residuals and trajectory output in the run.
The above listed Cards and their options can be included in the ``iisset_start`` file.
- Navigate to : ``/data/geodyn_code/RUNS/INPUTS`` and edit the ``iisset_start`` file.
- Search on ``/OBSVU``
     - Should be: ``OBSVU 4``
     - Outputs residual printouts to iieout
- Search on ``/RESIDU``
    - Should be: ``RESIDU12``
    - outputs residual file (binary)
- Search on ``/ORBFIL``
    - Should be: ``ORBFIL2 31       9806701     190420211800.0  190422 24200.00 .100000D+01``
    - Outputs trajectory file (binary)
- Search on /ORBTVU
    - Should be: ``ORBTVU1 6302610650630180000. 650708000000. 3600.``
    - Outputs trajectory file printouts as ascii files.


 


