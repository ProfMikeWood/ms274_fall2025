# California Upwelling

This modeling project is an example to demonstrate the approximate scope, files, and organization of a class project for MS 274.

## Project Description
In this project, I will investigate the effects of coastal wind on upwelling in California. In particular, I will investigate the following science question:

*How do winds affect ocean temperature in the California Current?*

To investigate this question, I will construct a model spanning the coast of California and run my model simulation for one year in two experiments: a run with wind and a run without wind. I will run my experiment during the year 2008 which is a year of strong upwelling as shown in the [BEUTI Index](https://ecowatch.noaa.gov/thematic/upwelling). I anticipate that the model with wind will have colder surface ocean temperatures than the model without wind because the ocean will be more stratified without the wind forcing.

For initial conditions, I will use the state of the ECCO Version 5 Model in January of 2008. Similarly, I will construct boundary and external forcing conditions for this model from the ECCO Version 5 model output. To analyze the results, I will create a timeseries of temperature in the Monterey Bay area in the two models and investigate differences through time. For visualization, I will create a movie of temperature differences between the model with wind and the model without wind. 


## Reproducing Model Results

*Note for MS274: The following section outlines possible steps that may be included in your README for reproducibility. When designing your own steps, be sure to consider which of the steps below pertain to your model and update/modify accordingly.*

The following steps outline how to construct the model files, configure and run the model, and assess the model results.

### Step 1: Create the Model Files
Several input files need to be created to run the model. Generate the following list of files using the notebooks indicated in paratheses:
- Model Grid (notebooks/Creating the Model Grid.ipynb)
- Bathymetry (notebooks/Creating the Bathymetry.ipynb)
- Initial Conditions (notebooks/Creating the Initial Conditions.ipynb)
- External Forcing Conditions (notebooks/Creating the External Forcing Conditions.ipynb)
- Boundary Conditions (notebooks/Creating the Boundary Conditions.ipynb)
The model files should be placed into the  `input` directory.

### Step 2: Add files to the computing cluster
Once the input files have been created, the model files can be transferred to the computing cluster. Begin by cloning a copy of [MITgcm](https://github.com/MITgcm/MITgcm) into your scratch directory and make a folder for the configuration, .e.g.
```
mkdir MITgcm/configurations/ca_upwelling
```
Then, use the `scp` command to send the `code`, `input`, and `namelist` directories to your configuration directory. 

### Step 3: Compile the model
Once all of the files are on the computing cluster, the model can be compiled. Make a `build` directory in the configuration directory and run the following lines:
```
../../../tools/genmake2 -of ../../../tools/build_options/darwin_amd64_gfortran -mods ../code -mpi
make depend
make
```

### Step 4.1: Run the model with wind
After the compilation is complete, run the model with the wind. Move to the run directory, link everything from `input` and `code`, and the submit the job script:
```
sbatch mwood.slm
```

### Step 4.2: Run the model without wind
Next, run the model without wind to complete the experiment. Again, link everything from `input` and `code` to a directory called `run_no_wind`. Then, edit the `data.exf` file to point to the modified wind files (see the Creating the External Forcing Conditions.ipynb notebook for details). Then, submit the job script again to rerun the model.

### Step 5: Analyze the Results
There are two notebooks provided for analysis:
1. Analyzing Model Results

   This notebook is provided to have a quick look at spatial and temporal variations in the temperature field in the model with wind. It also generates the visualization provided in the figures directory.
   
2. Answering the Science Question
   
   This notebooks provided some analysis plot to address the science question posed above.
