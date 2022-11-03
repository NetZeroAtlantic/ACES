# Introduction

Hi! Welcome to the ACES-Temoa repository. This repo contains the code behind
the Atlantic Canada Energy System (ACES) Model, which is based off the [Temoa](https://temoacloud.com/)
modelling suite. In fact, this repository is simply a clone of the Temoa [GitHub repository](https://github.com/TemoaProject/temoa)
repository. This was done to allow us to make code
additions specific to the Atlantic Canadian context that did not exist in the core Temoa code. 


# Overview

The 'ACES' branch is the current master branch of
Temoa.  The four subdirectories are:

1. `temoa_model/`
Contains the core Temoa model code.

2. `data_files/`
Contains SQLite input data (DAT) files for ACES. 

3. 'data_documentation/'
Contians documentation for database in 'data_files/'

4. `data_processing/`
Contains several modules to make output graphs, network diagrams, and 
results spreadsheets.

5. `tools/`
Contains scripts used to conduct sensitivity and uncertainty analysis. 
See the READMEs inside each subfolder for more information.

6. `docs/`
Contains the source code for the Temoa project manual, in reStructuredText
(ReST) format.

## Creating a ACES Environment

ACES requires several software elements, and it is most convenient to create 
a conda environment in which to run the model. To begin, you need to have conda 
installed either via miniconda or anaconda. Next, download the environment.yml file, 
and  place in a new directory named 'ACES' Create this new directory in 
a location where you wish to store the environment. From the command line:

```$ conda env create```

Then activate the environment as follows:

```$ source activate ACES```

This new conda environment contains several elements, including Python 3, a 
compatible version of Pyomo, matplotlib, numpy, scipy, and two free solvers 
(GLPK and CBC). A note for Windows users: the CBC solver is not available for Windows through conda. Thus, in order to install the environment properly, the last line of the 'environment.yml' file specifying 'coincbc' should be deleted.

To download the Temoa source code, either clone the repository or download from GitHub 
as a zip file.

## Running ACES

To run ACES, you have a few options. All commands below should be executed from the 
top-level 'ACES' directory.

**Option 1 (full-featured):**
Invokes python directly, and gives the user access to 
several model features via a configuration file:

```$ python  temoa_model/  --config=temoa_model/config_sample```

Running the model with a config file allows the user to (1) use a sqlite 
database for storing input and output data, (2) create a formatted Excel 
output file, (2) specify the solver to use, (3) return the log file produced during model execution, (4) return the lp file utilized by the solver, and (5) to execute modeling-to-generate alternatives (MGA). Note that if you do not have access to a commercial solver, it may be faster run cplex on the NEOS server. To do so, simply specify cplex as the solver and uncomment the '--neos' flag.


**Option 2 (basic):**
Uses Pyomo's own scripts and provides basic solver output:

```$ pyomo solve --solver=<solver> temoa_model/temoa_model.py  path/to/dat/file```

This option will only work with a text ('DAT') file as input. 
Results are placed in a yml file within the top-level 'temoa' directory.


**Option 3 (basic +):**
Copies the relevant Temoa model files into an executable archive 
(this only needs to be done once):

```$ python create_archive.py```

This makes the model more portable by placing all contents in a 
single zipped file. Now it is possible to execute the model with the 
following simply command:

```$ python temoa.py  path/to/dat/file```

For general help use --help:

```$ python  temoa_model/  --help```


More information on ACES model can be found on the official NetAtlanticZero webpage 
(https://netzeroatlantic.ca/acesmodel/about)


The rest of the document contains the source files necessary to generate the Temoa documentation in [ReST](https://en.wikipedia.org/wiki/ReStructuredText) format.

## Required Software

The required software elements to produce the documentation are included in the Temoa [environment file](https://github.com/TemoaProject/temoa/blob/energysystem/environment.yml).

The only additional software element required is LaTex. On a Mac, [MacTeX](https://www.tug.org/mactex/mactex-download.html) is a good option, and must be installed manually. [MiKTeX](https://miktex.org/download) is also available across platforms and is installed manually. On a Windows machine, MikTeX can also be installed through `conda` with the following command:

```$ conda install -c conda-forge miktex```

## Producing documentation
The Temoa documentation draws from a couple of sources: (1) the static descriptions of model elements included in [Documentation.rst](source//Documentation.rst), and (2) the doc strings
embedded in [temoa_rules.py](../temoa_model/temoa_rules.py) that document the objective function and constraints. Sphinx retrieves these doc strings and generates LaTeX-formatted equations in the "Equations" section of the documentation.


From this folder, execute the following to generate the html documentation:

```$ make html```

To generate the PDF documentation, from the same folder, execute the following:

```$ make latexpdf```

Sometimes this automatic PDF generation fails. If that is the case, navigate to `/tmp/TemoaDocumentationBuild/` and manually generate the pdf:

```$ pdflatex toolsforenergymodeloptimizationandanalysistemoa.pdf```





