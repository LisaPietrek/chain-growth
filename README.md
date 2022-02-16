# hierarchical-chain-growth
Grow ensembles of disordered biomolecules from fragment libraries

## Overview
Algorithm to assemble full-length chains of disordered proteins / regions from short overlapping fragments. 
As iinput to perform the hierarchical chain growth you need the sequence of the desired IDP and provide a MD fragment library

We provide example some scripts:
- to prepare the fragments to generate the MD fragment libraries using amber tleap
- (to run REMD simulations of the fragments on a SLURM-based cluster)
- to run hcg or rhcg + corresponding weights for a truncated tau K18

## Installation

## Inputs
- the sequence or topology file of the desired IDP/IDR as .fasta or .pdb, respectively. If you use .fasta make sure the fasta file contains a "header" in row 1 beginning with ">".
- a MD fragment library. You may prepare the library following the examples given in this repository.
- if you want to perform RHCG: weights for the individual fragments (e.g., from ensemble reweighting against experimental data reporting on _local_ structure properties - such as chemical shifts)

## Run tests

## REMD simulation of short fragments to generate MD fragment libraries
_[NOTES!!]_
- example scripts for SLURM-based cluster
- need to be adapted to cluster setup and again benchmarked
- REMD submit scripts for individual fragment - ideally write loop cover all fragments

## References
Hierarchical Ensembles of Intrinsically Disordered Proteins at Atomic Resolution in Molecular Dynamics Simulations

Lisa M. Pietrek, Lukas S. Stelzl, and Gerhard Hummer
Journal of Chemical Theory and Computation 2020 16 (1), 725-737, https://pubs.acs.org/doi/abs/10.1021/acs.jctc.9b00809


Global Structure of the Intrinsically Disordered Protein Tau Emerges from its Local Structure

Lukas S. Stelzl, Lisa M. Pietrek, Andrea Holla, Javier S. Oroz, Mateusz Sikora, Jürgen Köfinger, Benjamin Schuler, Markus Zweckstetter, Gerhard Hummer
bioarxiv, http://dx.doi.org/10.1101/2021.11.23.469691
