---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 3.2: Singularity Examples on OSG Connect
============================================================

Background
----------

The previous exercise (3.1) illustrated the idea of using containers, but 
didn't run a specific software program. In this exercise, you can choose one 
or more sample programs to run using a container. 

Setup
-----

1. Make sure you are logged into `login05.osgconnect.net`. 

1. To get an idea on what container images are available on the OSG, 
take a look at [this list](https://support.opensciencegrid.org/support/solutions/articles/12000073449-view-existing-ospool-supported-containers)
or the names listed at the directory path `/cvmfs/singularity.opensciencegrid.org/opensciencegrid`.  

> If you want to use your own container on the OSG, ask one of the school staff
> about how to do so. 

1. For all three examples, make sure you have the Singularity `requirement` from the 
[previous exercise](../part3-ex1-singularity)) in your submit file, and you'll 
still be using the `+SingularityImage` flag. 

Option 1: R
---------------------

1. Create the following R script, called `simple.R`:

		:::file
		#!/usr/bin/env Rscript
		
		inputs = c(5, 19, 108, 42, 77)
		results = sum(inputs)
		print(results)

1. In your submit file, set the R script as the executable: 

		:::file
		executable = simple.R

	We can use the R script directly (without another wrapper script) because 
	we included the header `#!/usr/bin/env Rscript` at the top of our script 
	file. This is a special indicator that the script should be run using 
	the `Rscript` program. 

1. Choose one of the existing R containers to run the job: 

		:::file
		+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/opensciencegrid/osgvo-r:4.0.2"

1. Submit the job and check the standard output file when it completes. 


Option 2: GROMACS
---------------------

> Example taken from [alchemistry.org](http://www.alchemistry.org/wiki/GROMACS_4.6_example:_Direct_ethanol_solvation_free_energy)

1. Create the following bash script, called `run_gromacs.sh`:

		:::file
		#!/bin/bash
		
		gmx grompp -f ethanol.0.mdp -c ethanol.gro -p ethanol.top -o ethanol.0.tpr -maxwarn 4
		
1. Unzip the input files, and move them to the current directory: 

		:::console
		username@login $ tar -xzf /public/osgvs21/gromacs_input.tgz
		username@login $ cp gromacs_input/* ./

1. In your submit file, set the `run_gromacs.sh` script as the executable: 

1. Add the input files to the appropriate line in the submit file. 

1. Choose the GROMACS container to run the job: 

		:::file
		+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/opensciencegrid/osgvo-gromacs:latest"

1. Submit the job and check the standard output file when it completes. 

> Those who want a little extra challenge - this example is meant to be run 9 times, 
> per the tutorial link above. Read through the first paragraph of the tutorial and 
> try to modify this submit file (and input files) so that it runs 9 jobs, each 
> with a different `init-lambda-state` value in the `ethanol.X.mdp` file. 

Option 3: Cowsay
---------------------

One final (fun) example - running the program "cowsay" from a container. 

1. Create the following R script, called `cowsay.sh`:

		:::file
		#!/bin/bash
		
		cowsay "Computing is Fun"

1. Make `cowsay.sh` your executable. 

1. Choose the `lolcow` container to run the job: 

		:::file
		+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/sylabsio/lolcow:latest"

1. Submit the job and check out the output when it completes! 

> Note: it's possible to run jobs using containers (like this one) *without* using 
> a wrapper script. If we wanted to run the `cowsay` program, directly as the executable, 
> it would look like this: 
> 
> 	:::file
> 	executable = /usr/games/cowsay
>	arguments = "Computing is Fun"
>	transfer_executable = false
>		
>	+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/sylabsio/lolcow:latest/"
>
> Note that this example is slightly different than our previous scripts; here 
> we're using cowsay directly from the container. Since it's not a separate executable, 
> we need to add the `transfer_exectuable = false` option 
