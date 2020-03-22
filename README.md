# COVID-19-ICU

This tool implements a SIRD model applied to COVID-19 spreading. 

**WE ARE CURRENTLY IMPROVING THE API; THE SOURCE CODE WILL BE PUBLISHED SOON**.

## The model

The model simulates the variation over time of the following variables: the number of susceptible individuals (*S*); the number of infected individuals (*I*); the number of individuals who recovered from the disease (*R*); the number of deceased patients (*D*). The model consists in the following system of coupled ordinary differential equations:
- ![alt text](https://latex.codecogs.com/gif.latex?%5Cdot%20S%20%3D%20-%5Cbeta%20%5Ccdot%20S%20%5Ccdot%20I "dS/dt = -beta*S*I")
- ![alt text](https://latex.codecogs.com/gif.latex?%5Cdot%20I%20%3D%20%5Cbeta%20%5Ccdot%20S%20%5Ccdot%20I%20-%20%5Cgamma%20%5Ccdot%20I%20-%20%5Cdelta%20%5Ccdot%20I "dI/dt = beta*S*I - gamma*I - delta*I")
- ![alt text](https://latex.codecogs.com/gif.latex?%5Cdot%20R%20%3D%20-%5Cgamma%20%5Ccdot%20I "dR/dt=delta*I")
- ![alt text](https://latex.codecogs.com/gif.latex?%5Cdot%20D%20%3D%20-%5Cdelta%20%5Ccdot%20I "dD/dt=delta*I")

The user must provide time-series of the number of infected, recovered and deceased patients. Then, four parameters are automatically calibrated against observed time-series:
- *S*(0), i.e., the number of susceptible individuals in the population at *t*=0;
- the transmission rate ![alt text](https://latex.codecogs.com/gif.latex?%5Csmall%20%5Cbeta "beta");
- the recovery rate ![alt text](https://latex.codecogs.com/gif.latex?%5Csmall%20%5Cgamma "gamma");
- the fatality rate ![alt text](https://latex.codecogs.com/gif.latex?%5Csmall%20%5Cdelta "delta").

The rest of the initial state of the system (i.e., *I*(0), *R*(0), and *D*(0)) are automatically determined by the input time-series.

## Simulating lockdown events

Our tool allows the simulation of lockdown events. An event is modeled as a pair (days, ![alt text](https://latex.codecogs.com/gif.latex?%5Csmall%20%5Calpha "alpha")) where:
- days is the number of days after the beginning of the time-series, in which the lockdown is applied;
- ![alt text](https://latex.codecogs.com/gif.latex?%5Csmall%20%5Calpha "alpha") is a scaling factor for the ![alt text](https://latex.codecogs.com/gif.latex?%5Csmall%20%5Cbeta "beta") parameter, simulating a slower transmission rate due to limited social interaction.

The user can provide a list of events, in order to simulate the effect of multiple or pulsed lockdowns.

## Technical details
- scipy's LSODA is used for ODE integration; 
- [FST-PSO](https://www.sciencedirect.com/science/article/abs/pii/S2210650216303534) is the Computational Intelligence algorithm used for parameters calibration;
- the objective function used for the calibration is the cumulative RMSE error of the model with respect to the target time-series.

## Further information

For information about the modeling approach, please contact Prof. Daniela Besozzi (daniela.besozzi@unimib.it).

For details about the python implementation, please contact Dr. Marco S. Nobile (m.s.nobile@tue.nl)

