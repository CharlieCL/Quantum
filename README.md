# Quantum Solver for Black-Scholes Equation

# Black-Scholes Equation in Fintech

The Black-Scholes equation (also known as the Black-Scholes-Merton equation) is a deterministic, second-order partial differential equation (PDE) that governs the price evolution of financial derivatives over time. Developed by economists Fischer Black, Myron Scholes, and Robert Merton in 1973, the equation is a cornerstone of modern financial engineering and quantitative modeling.

The fundamental financial insight behind the equation is riskless delta hedging. In an idealized, frictionless market, a trader can perfectly replicate the payoff of an option by dynamically buying and selling a specific fraction of the underlying asset (the asset's "Delta"). Because this continuously adjusted portfolio completely eliminates market randomness and uncertainty, it becomes effectively risk-free. Under the principle of no-arbitrage, the rate of return on this riskless portfolio must precisely equal the risk-free interest rate. The Black-Scholes PDE mathematically encapsulates this exact equilibrium, balancing the derivative's time decay (Theta) and its sensitivity to asset price changes (Gamma) against the risk-free rate.

* Key Applications

** Option Pricing: **
It is primarily utilized to calculate the exact theoretical fair price of European-style call and put options (derivatives that can only be exercised at their fixed expiration date).

** Risk Management and "The Greeks": **
Financial institutions use the model to compute market sensitivities known as "the Greeks." These parameters (Delta, Gamma, Vega, Theta, Rho) quantify how much an option's price changes relative to variables like underlying asset price, volatility, and time, allowing firms to hedge large portfolio exposures.

** Corporate Finance & Private Markets:** 
The model is adapted to value non-standard corporate financial instruments that possess option-like characteristics, such as employee stock options, convertible debt notes, warrants, and early-stage venture capital funding milestones.

** Foundational Benchmark:** 
It acts as the mathematical baseline for more complex modern financial models that relax its rigid assumptions, leading to advanced derivatives modeling involving stochastic, local, or fractional volatility.

# TQC-VQE PDE Solver for the Black-Scholes Equation
The Triangle Quantum Circuit - Variational Quantum Eigensolver (TQC-VQE) framework offers a powerful hybrid quantum-classical pipeline to solve complex partial differential equations, such as the Black-Scholes equation. Rather than relying entirely on classical computing clusters that scale poorly with high-dimensional options, the TQC-VQE handles the problem through the following specialized architectural layers:

A. Finite Difference Method (FDM) Discretization

To map the continuous, time-dependent Black-Scholes PDE onto a quantum processor, the solver first converts it into a discrete algebraic system using the Finite Difference Method (FDM). The domain of the underlying asset price (
) is divided into a structured grid of discrete points. The continuous partial derivatives (
 
 and 
 
) are replaced by discrete algebraic differential approximations (such as central differences). This reduction transforms the financial differential equation into a matrix eigenvalue problem formatted as 
.

B. Hamiltonian System Mapping

Because standard Variational Quantum Eigensolvers find eigenvalues of physical energy systems, the discrete matrix system (
) derived from the Black-Scholes grid is mapped into a corresponding Hamiltonian operator (
). In this context, calculating the fair price distribution of the financial option corresponds directly to finding the minimum eigenvalue—or ground state energy—of the defined Hamiltonian system.

C. Universal Ansatz via Triangle Blocks

To bypass the severe optimization bottlenecks (such as high gate depths and optimization dead-ends) common in general-purpose quantum circuits, the TQC-VQE relies on a Universal Ansatz built from hierarchically structured Triangle Blocks.These triangle blocks are mathematically tailored from triangle qubit channels.They leverage Channel-State Duality (CSD) and the Choi-Jamiołkowski isomorphism to represent unital quantum channels through Bell diagonal states.This dual configuration allows the circuit to function as a highly compact, expressive probability generator.It constructs an optimized quantum state using a lean arrangement of parameterized single-qubit rotation gates (
) and entangling CNOT gates that mimic the structure of the discretized financial operator.

D. The Closed Quantum-Classical Loop

The solver executes using an automated, iterative feedback loop:Quantum Measurement: The Parameterized Quantum Circuit (PQC) prepares a trial wave function state based on an initial set of parameters (
). The quantum hardware measures the expectation value of the Black-Scholes Hamiltonian matrix.Classical Optimization: This calculated energy expectation is fed into a classical optimizer as an objective cost function.Parameter Update: The classical algorithm computes the next gradient step, adjusts the parameters (
), and feeds the new angles back into the quantum hardware.This continuous loop rapidly drives the system toward a flat convergence curve, yielding a high-precision ground-state configuration that translates directly back into the optimized option price values across the financial grid.
