# 2D Boundary Integral Particle Initialization (BIPI) algorithm for Smooth Particle Hydrodynamics (SPH)

This free 2D SPH particle packing softare helps with particle initialization in Smoothed Particle Hydrodynamics (SPH) simulations, particularly for complex geometries without requiring virtual particle layers.

Note: *Current version is only avaible for 2D geoemtries* - All future updates, and code releases will be made avilable on this repository

---

## Features

# BIPI softare:
- **Robust Particle Packing**: Initialize particles efficiently, minimizing errors caused by improper particle distribution near boundaries.
- **Boundary Integral SPH Compatibility**: Specifically tailored for Boundary Integral SPH models, supporting simulations involving complex geometries and dynamic boundaries.
- **Reduced Computational Load**: Avoids the need for generating virtual particles by relying on boundary integrals, saving both time and memory.
- **Fast Algorithm for cartesian background grid**: If particles are initialized from a background cartesian grid - a preferred way in SPH community - the algorithm provides a fast methodology to achieve an acceptable initial particle distribution. The free software only allows for particle generation on cartesian grid.

# Python particle generation script file:
- A python script is released to allow for generating particles on cartesian grid, using the 2D boundary mesh file generated from Siemens NX software.
- Multiple example boundary data files are provided for testing different geoemtries, all of which are also used in the research paper related to this code (see end of file for paper).

---

## Getting Started

### Prerequisites

Before using this repository, ensure you have the following:

- **Python (v3.8+)**: For sgenerating particles from NX boundary mesh files of NX.
- **Windows**: The software is currently made avalable as an executable and runs on Windows.


## Usage


### 1. Obtain boundary element data and initial particle distribution using the BIPI2D software
The boundary element data requires node coordinates of each element, and the nodes need to be in the same sense such that the surface normal constructed from them points outwards.

Note: Check the readme file in Folder NxtoParticleGeneration to see how to use the sample python script for generating initial particle distribution on background grid by using the boundary mesh format of Siemens NX mesh, automatically. The sample script will need to be modified to generate particle distribution on artesian grid for other boundary mesh formats of different commercial and open source softwares.

#### i) The boundary data is provided to the software through data_geo_config/input_bulkBdryEdge.dat
For a unit square the boundary data format the BIPI-SPH code accepts is as follows:
```bash
2  0.0  0.0  0.1  0.0
2  0.1  0.0  0.1  0.1
2  0.1  0.1  0.0  0.1
2  0.0  0.1  0.0  0.0
```
Here, 
a) Each row represents an element
b) The first column represents the 2D edge type (which is irrelevant here, but is relevant for our BISPH code base for Boundary type specification)
c) The second and third column represent coordinates of the first vertex of an edge
d) The fourth and fifth column represent  coordinates of the second vertex of an edge

#### ii) The initial particle distribution is provided to the software through data_geo_config/input_particles_cartesianMesh.dat
For initial particle distribution, particle data needs to be provided in the following format:
```bash
3  0.025  0.025  0.025
3  0.075  0.025  0.025
3  0.075  0.075  0.025
3  0.025  0.075  0.025 
```
Here,
a) Each row represents a particle
b) The first and second column represents the coordinates of the particle
c) The third column represents the volume of the particle

#### iii) Provide details of boundary data through data_geo_config/input_param_SimSize.dat
```bash
0  4  0.4  0.0  0.0
```
Here,
a) The first number and the last two numbers are not important for the BIPI2D software released here and can be left as 0
b) The second element represents the number of particles ( use a higher approximate if the true value is unavaiable)
c) The third element represents the total length of the boundary ( use a higher approximate if the true value is unavaiable)

### 2. Running the Algorithm
To run the BIPI algorithm on a sample geometry update the data_geo_config/config_BIPI.txt file and run the executable "BIPI-algorithm.exe" of BIPI2D software provided.
#### Parameters of config_BIPI.txt file
```bash
save_packing 1 
dx_r 0.025
pack_step2a 5000
pack_step2b 0.6
pack_step2c 1000
shorten_step2a 1
shorten_step2c 1
hsml_const 0.05
PSTCoeff 0.5
nnps 2
nnes 2
edge_to_dx_ratio 10
```  
Here
- save_packing: This lets user view particle plot and evolution of the average Total Particle Displacement($TPD_avg$) and average Concentration gradient ($|\nabla \mathcal{C}| \_{avg}  $) plots on tecplot. The files are saved in fodler data_packing. The value 1 implies every iteration is saved, value 100 implies every 100 iteration is saved. Use 0 if intermediate particle positions during the algorithm and evlution of $TPD_avg$ and $|\nabla \mathcal{C}| \_{avg}  $
- dx_r: particle spacing
-  pack_step2a : Total number of iterations to run step2a of BIPI
-  pack_step2b : value of $\frac{k_2b}{dx_r}$ for step2b of BIPI
-  pack_step2c : total number of iterations to run step2c of BIPI
-  shortern_step2a: 1 for True, 0 for False - if True 1% criteria for step2a is used, and smaller of shortern_step2a and pack_Step2a is then used as stopping criteria for step2a
-  shortern_step2a: 1 for True, 0 for False - if True 1% criteria for step2c is used, and smaller of shortern_stepca and pack_Step2c is then used as stopping criteria for step2c
-  hsml_const: value of smoothing length
-  PSTCoeff: Value of  $\mathcal{J}$ in $D_a = \mathbb{J} h^2$
-  nnps : Nearest neighbor particle search, use 1 for direct search and 2 for a cell-linked search
-  nnes : Nearest neighbor edgse search for boundary particles, use 1 for direct search and 2 for a cell-linked search
-  edge_to_dx_ratio : this is the ratio of $\frac{dx_r}{dx_s}$ - the algorithm can divide the edges into additional linear elements to maintain the necessary $\frac{dx_r}{dx_s}$ ratio

### 3. Packed particle configuration
The packed particle configuration is saved as data_geo_config/input_PP.dat, the format of the output data file is the same as data_geo_config/input_particles_cartesianMesh.dat
```bash
3  0.025  0.025  0.025
3  0.075  0.025  0.025
3  0.075  0.075  0.025
3  0.025  0.075  0.025 
```
----------

## Examples

Multiple boundary data files for generating particles is provided in the folder NxtoParticleGeneration/Test_datafiles

----------

## License

This repository is licensed under the [MIT License](https://opensource.org/license/mit).

----------

## Citation

If you use the BIPI algorithm in your research, please cite:
```bash
@article{BIPI-SPH Algorithm,
  author = {Parikshit Boregowda, Gui-Rong Liu},
  title = {A boundary integral based particle initialization algorithm for Smooth Particle Hydrodynamics},
  journal = {},
  year = {2024},
  doi = {10.48550/arXiv.2403.07779}
}
```

----------
