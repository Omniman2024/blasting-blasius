# blasting-blasius

This repository contains a numerical solution for the **Blasius Equation**, which describes the velocity profile of a laminar boundary layer over a flat plate. The project implements a **Shooting Method** using **Bisection** to solve the third-order non-linear Ordinary Differential Equation (ODE).

## Overview

The Blasius equation is a fundamental result in fluid mechanics derived from the steady, 2D incompressible **Navier-Stokes equations**. Under boundary layer assumptions ($\delta \ll x$), these equations are transformed into a single third-order non-linear ODE using a similarity variable $\eta$.

### 1. The Physics: From Navier-Stokes to Blasius
The transformation utilizes the **Stream Function** ($\psi$) and the **Similarity Variable** ($\eta$):

$$f''' + \frac{1}{2} f f'' = 0$$

Where:
* $f(\eta)$ is the dimensionless stream function.
* $\eta = y \sqrt{\frac{U_\infty}{\nu x}}$ represents the scaled vertical distance from the plate.



### 2. The Boundary Value Problem (BVP)
The solver satisfies three critical physical boundary conditions:
* **No-Slip Condition**: $f'(0) = 0$ (Fluid velocity at the wall is zero).
* **No-Penetration Condition**: $f(0) = 0$ (The plate surface is solid).
* **Free-Stream Condition**: $f'(\eta) \to 1$ as $\eta \to \infty$ (Velocity matches external flow at the edge of the layer).

### 3. The RK4 Method
The RK4 method is a numerical integrator that handles the "marching" process. Since the Blasius equation is a differential equation, we know the "rules" of how the fluid moves, but we don't have a simple formula for its final position. It takes the current state of the fluid at the wall and moves it forward in tiny, discrete steps (h). Instead of just looking at the slope at the current point, it "peeks" ahead at the midpoint and the end of the step to calculate four different slopes (k1​ through k4​). Finally it takes a weighted average of these slopes to find the most accurate "path" for the fluid, ensuring the error stays extremely low.

### 4. The Bisection Method (Shooting)

The Bisection method is an iterative root-finding algorithm used because we are missing one critical piece of information at the wall: the initial shear stress, f′′(0). We know the velocity must reach 1.0 far away from the plate, but we don't know what "angle" to start at to get there. It picks a high guess and a low guess for the starting shear stress. It then repeatedly cuts the interval in half (bisects it) based on whether the resulting velocity was too high or too low. After several iterations, it "homes in" on the exact value of f′′(0)≈0.332 that allows the RK4 worker to perfectly reach the free-stream velocity.

## Features

* **Numerical Integration:** Uses the **4th-Order Runge-Kutta (RK4)** method for high-accuracy state transitions.
* **Shooting Method:** Implements a **Bisection algorithm** to iteratively find the "Blasius Constant" ($f''(0) \approx 0.332$).
* **Physical Transformations:** Converts dimensionless similarity variables ($\eta$, $f'$) into real-world units (meters, m/s).
* **Visualizations:**
    * **Boundary Layer Growth:** Physical velocity profiles ($u$ vs. $y$) at various downstream positions ($x$).
    * **Shear Stress Decay:** Variation of wall shear stress ($\tau_w$) along the plate length ($x$).

## Key Results

### The Blasius Constant
The solver converges to the wall shear stress factor:
$$f''(0) \approx 0.33206$$
This value is critical for calculating skin friction drag in aerodynamics and hydrodynamics.

### Physical Insights
1.  **Velocity Profile:** The fluid follows the no-slip condition at the wall ($u=0$) and reaches 99% of the free-stream velocity at $\eta \approx 4.91$.
2.  **Boundary Layer Growth:** The thickness of the layer ($\delta$) grows proportionally to $\sqrt{x}$.
3.  **Friction Decay:** The wall shear stress ($\tau_w$) is highest at the leading edge and decreases as $1/\sqrt{x}$ further downstream.

## Dependencies
```
pip install numpy matplotlib
```
