# TTW Artifacts

[![Binder](https://mybinder.org/badge_logo.svg)][ttw_artifact_binder]

[ttw_artifact_binder]: https://mybinder.org/v2/gh/romain-jacob/TTW-Artifacts/master


This repository describes all the public artifacts related to the Time-Triggered Wireless Architecture project, presented in the following paper. This paper serves as reference for the sections mentioned in this file.

> **Time-Triggered Wireless Architecture**  
Romain Jacob, Licong Zhang, Marco Zimmerling, Jan Beutel, Samarjit Chakraborty, Lothar Thiele   
ECRTS 2020  
[ [10.4230/LIPIcs.ECRTS.2020.19](https://doi.org/10.4230/LIPIcs.ECRTS.2020.19) ]
[ [arXiv (submitted version)](https://arxiv.org/abs/2002.07491) ]  

## How to replicate the results from the paper?

+ **Figure 3.**   
The time and energy models for TTnet (Section 3.3) are implemented in Python. A Jupyter notebook is provided to present the models' APIs and produces Figure 3. See [TTnet model](#ttnet-model) for details.
+ **Table 2, Figure 8.**  
Our TTnet implementation  (Section 3.2) is available in a dedicated repository. The raw data has been collected over multiple months on a wireless network testbed. As this is challenging to replicate, we provide the raw data we collected, as well as the processed data leading to the results in Table 2 and Figure 8. See [Reproducing the data processing](#reproducing-the-data-processing) for more details.
+ **Table 3, Figure 10.**  
Our implementation of the TTW Scheduler (Section 4) is available in a dedicated repository. The scheduler is implemented in Matlab and requires in addition the Gurobi solver. The code contains the configuration files required to replicate the results from Table 3. See [Run the TTW scheduler](#run-the-ttw-scheduler) for details.
+ **All plots**  
The plots presented in the paper have been generated using a Jupyter notebook which is included in this repository. See [Reproducing the plots](#reproducing-the-plots) for more details.

---

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [TTnet model](#ttnet-model)
- [Compile and run TTnet](#compile-and-run-ttnet)
- [Run the TTW scheduler](#run-the-ttw-scheduler)
- [Reproducing the data processing](#reproducing-the-data-processing)
- [Reproducing the plots](#reproducing-the-plots)

<!-- /TOC -->

<!-- ############################################### -->
## TTnet model
<!-- ############################################### -->

This repository contains all the information required to use the TTnet time and energy models (Section 3.3). This requires
+ The `/src` directory, which includes the Python source code
+ The Python modules listed in the `requirements.txt` file

The `ttnet_model.ipynb` notebook presents the model, illustrates the various functions computing the model equations, and shows some sample plots comparing the effects of different parameter values. This notebook can be run in two ways:
<!-- This notebook can be run directly in your web browser [using Binder](https://mybinder.org/) (it may take a few minutes to launch). -->

> **Dynamic execution in the browser**  
[![Binder](https://mybinder.org/badge_logo.svg)  
Run `ttnet_model.ipynb` in Binder.][ttnet_model_binder]

> **Static rendering**  
[![nbviewer](https://img.shields.io/badge/render-nbviewer-orange.svg)  
Visualize `ttnet_model.ipynb` with nbviewer.][ttnet_model_nbviewer]



<!-- # Links # -->
[ttnet_model_binder]: https://mybinder.org/v2/gh/romain-jacob/TTW-Artifacts/master?filepath=.%2Fttnet_model.ipynb
[ttnet_model_nbviewer]: https://nbviewer.jupyter.org/github/romain-jacob/TTW-Artifacts/blob/master/ttnet_model.ipynb
<!-- # Links # -->

<!-- ############################################### -->
## Compile and run TTnet
<!-- ############################################### -->

Our TTnet implementation (Section 3.2) is based on [Baloo](https://github.com/ETHZ-TEC/Baloo/tree/master), a design framework for network stacks based on synchronous transmissions. Instructions for setting up and running Baloo are described in detail in the [Baloo Wiki](https://github.com/ETHZ-TEC/Baloo/wiki).
The TTnet implementation is located under [`examples/baloo-ttnet`](https://github.com/ETHZ-TEC/Baloo/tree/master/examples/baloo-ttnet). This directory includes a README file that details the build and run commands, as well as information related to the TTnet implementation and how to run it.

Once you have completed the [setup of Baloo's toolchain](https://github.com/ETHZ-TEC/Baloo/wiki#getting-started), you can easily compile and run an example multi-mode TTnet application on the [FlockLab testbed](http://flocklab.ethz.ch/) by running the following commands

```bash
git clone git@github.com:ETHZ-TEC/Baloo.git
cd Baloo/examples/baloo-ttnet
make FLOCKLAB=1 all flocklab_test
# After the test has been run...
make flocklab_viz TESTID=<your-test-id>
```

The firmware uses scheduling tables that are actual outputs from the [TTW Scheduler](#run-the-ttw-scheduler), which are produced by solving the `simple_example` configuration (see details in the [TTW Scheduler repository][ttw_repo]).

<!-- ############################################### -->
## Run the TTW scheduler
<!-- ############################################### -->

Our implementation of the TTW Scheduler (Section 4) is located in its own GitHub repository
+ [GitHub repo: romain-jacob/TTW-Scheduler][ttw_repo]
+ [Zenodo archive: ![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3530665.svg)][ttw_zenodo]

The scheduler is implemented in [Matlab][1] and uses [Gurobi][2] to solve the MILP formulation. The [TTW Scheduler repository][ttw_repo] contains detailed information allowing to
+ Setup Matlab and Gurobi
+ Reproduce the evaluation of TTW minimal inheritance approach (Section 5.2)
+ Formulate your own scheduling problem by specifying applications, modes, etc.

> **--- /!\ --- Beware --- /!\ ---**  
While preparing for the release of these software, we noticed differences in the schedules produced by the Gurobi solver, depending on the version.  
To guarantee the reproducibility of the results from the evaluation of TTW minimal inheritance approach, you should use the following software versions (tested on Ubuntu 18.04 LTS)
+ Matlab 2019b (64-bit)
+ Gurobi 9.0.1

Although Matlab and Gurobi are both commercial software (which is not ideal), free academic and/or student licenses are currently available from the software vendors.

[1]: https://www.mathworks.com/products/matlab.html
[2]: https://www.gurobi.com/
[ttw_repo]: https://github.com/romain-jacob/TTW-Scheduler
[ttw_zenodo]: https://doi.org/10.5281/zenodo.3530665

<!-- Link and instruction to run the TTW Scheduler (Matlab (with versions) + Gurobi). Check what has been written for the thesis already (I did stuff for DRP, don't remember for TTW) -->


<!-- ############################################### -->
## Reproducing the data processing
<!-- ############################################### -->

This repository contains all the information required to re-run the data processing for the TTnet validation experiments (Section 5.1). This requires
+ The `/src` directory, which includes the Python source code
+ The Python modules listed in the `requirements.txt` file
+ The raw data from the FlockLab experiments (one .zip of ~70Mb), which is available [on Zenodo](https://doi.org/10.5281/zenodo.3530721) [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3530721.svg)](https://doi.org/10.5281/zenodo.3530721)

The `ttnet_model_validation.ipynb` notebook summarizes the whole procedure, describes the steps required to do the processing, downloads the raw data, and produces some visualizations of the processed data. This notebook can be run in two ways:
<!-- This notebook can be run directly in your web browser [using Binder](https://mybinder.org/) (it may take a few minutes to launch). -->

> **Dynamic execution in the browser**  
[![Binder](https://mybinder.org/badge_logo.svg)  
Run `ttnet_model_validation.ipynb` in Binder.][ttnet_model_validation_binder]

> **Static rendering**  
[![nbviewer](https://img.shields.io/badge/render-nbviewer-orange.svg)  
Visualize `ttnet_model-validation.ipynb` with nbviewer.][ttnet_model_validation_nbviewer]

<!-- # Links # -->
[ttnet_model_validation_binder]: https://mybinder.org/v2/gh/romain-jacob/TTW-Artifacts/master?filepath=.%2Fttnet_model-validation.ipynb
[ttnet_model_validation_nbviewer]: https://nbviewer.jupyter.org/github/romain-jacob/TTW-Artifacts/blob/master/ttnet_model-validation.ipynb
<!-- # Links # -->

<!-- [![Binder](https://mybinder.org/badge_logo.svg)][ttw_artifact_binder] and open the `ttnet_model_validation.ipynb` notebook; or use this
[direct link.][ttnet_model_validation_binder]


[![Binder](https://mybinder.org/badge_logo.svg)][ttw_artifact_binder] and open the `ttnet_model_validation.ipynb` notebook;

or use this
-->

<!-- ############################################### -->
## Reproducing the plots
<!-- ############################################### -->

This repository contains all the information required to reproduce all the plots presented in the paper. This requires
+ The `/src` directory, which includes the Python source code
+ The Python modules listed in the `requirements.txt` file
+ The processed data, located in the `/data_processed` directory. Alternatively, you may re-run the processing from the raw data (see [Reproducing the data processing](#reproducing-the-data-processing)).

The `ttw_plots.ipynb` notebook produces, displays, and (optionally) saves all plots presented in the paper. It can be run in one of two ways:
<!-- This notebook can be run directly in your web browser [using Binder](https://mybinder.org/) (it may take a few minutes to launch).  
Alternatively, you can simply open a static rendering of the notebook. -->

> **Dynamic execution in the browser**  
[![Binder](https://mybinder.org/badge_logo.svg)  
Run `ttw_plots.ipynb` in Binder.][ttw_plot_binder]

> **Static rendering**  
[![nbviewer](https://img.shields.io/badge/render-nbviewer-orange.svg)  
Visualize `ttw_plots.ipynb` with nbviewer.][ttw_plot_nbviewer]



<!-- # Links # -->
[ttw_plot_binder]: https://mybinder.org/v2/gh/romain-jacob/TTW-Artifacts/master?filepath=.%2Fttw_plots.ipynb
[ttw_plot_nbviewer]: https://nbviewer.jupyter.org/github/romain-jacob/TTW-Artifacts/blob/master/ttw_plots.ipynb
<!-- # Links # -->


<!-- ############################################### -- >
<!-- ## Other related resources -->
<!-- ############################################### -- >
+ TTW poster
+ Previous paper (DATE)
+ Thesis chapter -->
