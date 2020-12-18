# Notes on the Project

## Atmospheric Density and Drag:
DRAG EQUATION _(add mathjax to add equation...)_  

Where,  
AD  acceleration due to atmospheric drag force  
CD is the satellite drag coefficient  
As is the cross-sectional area of the satellite  
ms is the mass of the satellite  
D is the-density of the atmosphere at the satellite position  
vr  is the velocity vector of the satellite relative to the atmosphere, and  
vr  is the magnitude of the velocity vector, vr .  

CD is treated as a constant in GEODYN, unless the parameter is being estimated in a data reduction run. The factor CD varies slightly with satellite shape and atmospheric composition. However, for any geodetically useful satellite, it may be treated as a satellite dependent constant.

The DRAG.f90 subroutine modifies drag application and /or requests estimation of drag coefficients.


## Regarding the Density Models in GEODYN:
**On Overfitting: [Sutton 2020, Private Communication]**  
When using orbital determination (OD) and estimating lots of parameters, residuals may not always show an obvious improvement even when a much better density model is used. In such a case, the deficiency of the original model is compensated by other estimated parameters, maybe CD, maybe other terms. This overfitting can lead to problems if you care about more than just the residuals, i.e., if you need to maintain a realistic CD in order to predict a future orbit. For instance, if you estimate CD = 100 with a horrible thermosphere model, that value may not persist so well into the future.

**Items to track when changing density models:**
 - Density at the satellite orbit
 - Residuals 
 - Trajectory (ephemeris) of the satellite
 - Cd
 - Other parameters that might are being estimated. 
    - What are the parameters that could be “soaking up” the thermosphere errors. To assess the thermosphere model, you’d want to keep these as few as possible to avoid overfitting. 
    - i.e. if you’re also estimating a solar radiation pressure coefficient, you’d want to know how it varies over time. 
   


## On the GEODYN Output
The GEODYN ouput is spread out across many files and which files needs to be used depends on what one is looking for.  Below is a breakdown of the ouput, its format, and what can be found in each file.  This is not an exhaustive list, and represents the files that are pertinent to this project (using geodyn for model validation).  
8

| File Name  | File type (readability)  | Contents  | GEODYN Designation  | documentation loc   |
|---|---|---|---|---|
| iieout  | ascii  |   |   |   |
| ascii_xyz  | ascii  |   |   |   |
| ascii_kep  | ascii |   |   |   |
| resid  | binary  |   |   |   |
| orbfil  | binary  |   |   |   |
| orbfil2  | binary  |   |   |   |
| densityfil  | ascii  |   |   |   |
| fort.9  | ascii  |   |   |   |
| punch   |  binary | ???   | ???  | TODO  |

[//]: <> (FIXME figure out what punch is)

 [comment]: # TODO