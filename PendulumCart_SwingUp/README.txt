README.txt -- Pendulum cart swing-up solvers

This directory contains three subdirectories:
    Chebyshev_Grid_Soln
    Piecewise_Linear_Soln
    GPOPS_2_Soln

Each subdirectory contains a script MAIN.m that, when run, will solve the trajectory optimization problem described below.

Chebyshev_Grid_Soln solves the trajectory on a Chebyshev grid, implicitly approximating the trajectory (both state and actuation) as a Chebyshev polynomial. Defects (for the collocation method) are computed by comparing the state derivatives as computed by two different methods:
    1) system dynamics (equations of motion)
    2) direct (analytic) differention of the chebyshev polynomials

Piecewise_Linear_Solution solves the trajectory on a uniform grid, approximating the trajectory (both state and actuation) as piecewise linear functions. Defects for collocation are computed by comparing the states, at all but the first time step:
    1) state at (k+1)
    2) 4th order Runge-Kutta integration step from state at (k)

GPOPS_2_Solution solves the trajectory optimization problem using a commercially available software: GPOPS 2. This code will not run unless you have installed GPOPS 2 into matlab and have a valid license.

All versions have a saved data file that contains the solution to the problem (as defined by the current parameters) solved on a fine grid. This data file can be used to initialize the starting guess for the solver.

There are a large number of parameters that can be adjusted, and in Chebyshev_Grid_Soln and Piecewise_Linear_Soln, ALL of the parameters are located in the file Set_Parameters.m. The GPOPS 2 solution has fewer parameters, and they are all found in MAIN.m.

All versions of the algorithm implement a few different solvers. If you have SNOPT installed, then you can use that. Otherwise (and by default)Chebyshev_Grid_Soln and Piecewise_Linear_Soln will call FMINCON and GPOPS 2 will use IPOPT. Within FMINCON you can choose from a few different algorithms as well.

System:
                      ________
		     |        | <==FORCE
_____________________|____O___|_________
                          \\
              ||           \\
     GRAVITY  ||            \\
              \/             \\
                             000
                             000
                   
    
CART - a point mass that is constrained to travel on a horizontal line. A control force is applied to it.

PENDULUM - a point-mass pendulum hangs from the cart, connected by a frictionless pin.

The goal is to start with the pendulum hanging down at rest, and apply a time-varying force such that after some finite time the pendulum is balanced vertically. Each version of the algorithm is set up to optimize the integral of force squared. Other options are available in Chebyshev_Grid_Soln.