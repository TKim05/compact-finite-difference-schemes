# High-Resolution Compact Finite-Difference Schemes

## Overview

This project explores compact finite-difference methods for numerical differentiation, with an emphasis on higher-order formulations and their computational properties.

The investigation uses Wolfram Mathematica to develop and explore numerical schemes, including pentadiagonal and heptadiagonal formulations.

## Research Objectives

- Investigate compact finite-difference approximations to derivatives.
- Explore higher-order schemes with different matrix structures.
- Examine how scheme parameters influence numerical behaviour.
- Compare alternative formulations through symbolic calculations and numerical visualisation.

## Methodology

### Compact Finite-Difference Methods

Compact schemes approximate derivatives using implicit relationships between neighbouring derivative values and function values.

Unlike conventional explicit finite-difference formulas, these methods typically require solving a system of linear equations.

### Pentadiagonal and Heptadiagonal Schemes

The project investigates higher-order compact formulations involving wider coupling between neighbouring derivative values.

### Symbolic and Numerical Investigation

Wolfram Mathematica is used for symbolic manipulation, numerical evaluation and graphical exploration of the schemes.

## Technologies

- Wolfram Mathematica
- Wolfram Language
- Numerical analysis
- Symbolic computation

## Project Structure

The `notebooks/` directory contains the selected Mathematica implementations and experiments.

The `results/` directory contains exported figures illustrating the numerical investigations.

## Research Context

Developed as part of a computational physics research internship investigating compact finite-difference schemes.

## Further Work

- Consolidate the experimental notebooks into reproducible implementations.
- Document numerical comparisons and parameter choices.
- Summarise findings from the different compact schemes.
