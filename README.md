# Hierarchical chain growth
Grow ensembles of disordered biomolecules from fragment libraries

## Overview
Algorithm to assemble full-length chains of disordered proteins and regions from short overlapping fragments [1]. 

As input to perform the hierarchical chain growth (HCG) you need the sequence of the desired IDP and provide a fragment
library. Fragment libraries are typically generated by molecular dynamics simulations. Experimental information on local 
conformations can be used to improve the quality of ensembles in reweighted hierarchical chain growth (RHCG) [2],
e.g., as we have used in our study of tau K18. 

In the examples folder, we use a truncated protein sequence of tau K18 to keep file sizes manageable. We provide
fragment libraries for this truncated sequence and example scripts to run HCG and RHCG.

We also provide our script-based workflow to generate conformational ensembles of a set of fragments. With these
Scripts, it is possible to efficiently sample the local conformations of disordered proteins such as tau or
alpha-synuclein. To accurately sample local structure, we use replica exchange molecular dynamics.

## Installation

Example installation in a new conda environment

```bash
conda create -n hcg_py3.7 python=3.7
conda activate hcg_py3.7
git clone https://github.com/bio-phys/hierarchical-chain-growth.git
cd hierarchical-chain-growth
pip install -e .
```

Python 3.6+ is required. This procedure was tested on Ubuntu 18.04.6. and Mac OS 12.0.1

## Inputs
- The sequence or topology file of the desired IDP/IDR as string (one letter amino acid code), .fasta, or .pdb, respectively. If you use .fasta make sure the fasta file contains a "header" beginning with ">".
- A MD fragment library. You may prepare the library following the examples given in this repository.
- If you want to perform RHCG: weights for the individual fragments (e.g., from ensemble reweighting against experimental data reporting on _local_ structure properties - such as chemical shifts)

## Example truncated tauK18

### Prepare fragments
- Use amber tleap to generate (end-capped) fragments of the desired length and with desired overlap.
- For "prepare_fragment_list_tleap.py" you need as input a .fasta or .pdb file to get the sequence of the IDP/IDR you want to grow.
- The script outputs a .txt file containing all the fragment sequences that you need to generate using tleap.
        - The output .txt file can be used as input for tleap.
- Here we show the procedure using the sequence of a truncated tauK18 as input (in the form of a fasta file).


### Run molecular dynamics simulations of the fragments

We have used replica-exchange molecular dynamics (REMD) simulations to extensively sample conformations for each fragment. The examples
folder provides our workflow to setup REMD simulations of fragments.

### Refine fragment ensembles with experimental data

In Ref [2] we show how to refine fragment ensembles with experimental data. The refined fragment ensembles can be used
for chain growth in an importance sampling framework. The BioEn package can be used to refine fragment ensembles [2,3].

### Run chain growth
The files "run_hcg.py" and "run_rhcg.py" show how to run HCG or RHCG for a truncated tau K18. These are simple Python scripts that
can be executed on the command line or submitted to a HPC cluster. Comments in "hcg_fct.py" explain different parameters that one can set for running chain growth and we refer to Ref 1 for more detailed discussions.
In the example we set the maximal number of full-length chains to grow, kmax, = 100 for test reason. We recommend setting kmax = 1000 and then running n task arrays
on a HPC cluster to grow an ensemble with n x1000 members. This can be done using, e.g., the "submit_hcg_TA.job" script we provide in this folder.
In our recent study [2] we found that ensembles with >=10000 members give meaningful ensemble averages.


## Testing

Run tests

```bash
  python -m pytest
```

## Web application of HCG

A free web application to grow ensembles of disordered proteins can be found here https://bio-phys.pages.mpcdf.de/hcg-from-library/. As input, we use a pre-sampled dimer fragment library (https://gitlab.mpcdf.mpg.de/MPIBP-Hummer/hcg-fragment-library.git).

## Branch "multiprocessing_hcg"

We uploaded a speed-up version of HCG to the branch “multiprocessing_hcg”. Here, fragment assembly steps in one level are run in parallel. This version of HCG is also run in our web application of HCG. NOTE: For now, only HCG can be run in this way.

### Single-stranded nucleic acid
This branch also contains a slightly adapted version of HCG (parallelized) to grow ensembles of disordered single-stranded nucleic acid (ssNA). For growing ssRNA polymers, we uploaded a pre-sampled fragment library here https://zenodo.org/records/8369324. Please refer to Ref 5 for more detailed information. 


## References
1 Hierarchical Ensembles of Intrinsically Disordered Proteins at Atomic Resolution in Molecular Dynamics Simulations.
Lisa M. Pietrek, Lukas S. Stelzl, and Gerhard Hummer,
Journal of Chemical Theory and Computation 2020 16 (1), 725-737, https://pubs.acs.org/doi/abs/10.1021/acs.jctc.9b00809

2 Global Structure of the Intrinsically Disordered Protein Tau Emerges from its Local Structure.
Lukas S. Stelzl, Lisa M. Pietrek, Andrea Holla, Javier S. Oroz, Mateusz Sikora, Jürgen Köfinger, Benjamin Schuler, Markus Zweckstetter, Gerhard Hummer,
Journal of American Chemical Society Au, 2022, 2, (3) 673–686, https://pubs.acs.org/doi/10.1021/jacsau.1c00536

3 Efficient Ensemble Refinement by Reweighting.
Jürgen Köfinger, Lukas S. Stelzl, Klaus Reuter, César Allande, Katrin Reichel, Gerhard Hummer,
Journal of Chemical Theory and Computation 2019 15 (5), 3390-3401, https://pubs.acs.org/doi/abs/10.1021/acs.jctc.8b01231

4 Structural ensembles of disordered proteins from hierarchical chain growth and simulation. Lisa M. Pietrek, Lukas S. Stelzl, and Gerhard Hummer, Current Opinion in Structural Biology,
2023, 78, 102501, https://doi.org/10.1016/j.sbi.2022.102501


5 Hierarchical Assembly of Single-Stranded RNA, preprint bioRxiv  September 2023, https://www.biorxiv.org/content/10.1101/2023.08.01.551474v2

