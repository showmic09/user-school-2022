---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 3.1: Use Singularity from OSG Connect
============================================================

Background
----------

Containers are another way to manage software installations. This guide shows how to use pre-existing containers to run jobs on the OSG.

One caveat for using containers: not all systems will support them. HTCondor has built-in features for using Docker and many OSG resources have Singularity installed, but they are not always available everywhere. 

Setup
-----

Make sure you are logged into `login04.osgconnect.net` (the OSG Connect submit server for this workshop).  For this exercise we will be using Singularity containers that are hosted by OSG Connect. 


Default Environment
-------------------

First, let's run a job without a container to see what the typical job environment is. 

1. Create a bash script with the following lines: 

		:::bash
		#!/bin/bash
	
		hostname
		cat /etc/os-release 
		gcc --version
	
	This will print out the version of Linux on the computer and the version 
	of `gcc`, a common software compiler. 

1. Copy a submit file from a previous OSG Connect job and edit it so that the 
script you just wrote is the executable. 

1. Submit the job and read the standard output file when it finishes. The output 
should indicate that the job ran on "CentOS Linux" and the version of GCC will be `4.x`.

Container Environment
---------------------

Now, let's try running that same script inside a container. 

1. On the OSG, containers are stored and run as Singularity container images, so the job needs to run on a server that has Singularity installed. Modify the submit file from the previous step and add (or replace) the following line: 

		:::file
		requirements = HAS_SINGULARITY == true

1. For this job, we will use the OSG Connect Ubuntu "Xenial" image. The `+SingularityImage` submit file option will tell HTCondor to use this container: 

		:::file
		requirements = HAS_SINGULARITY == true
		+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/opensciencegrid/osgvo-ubuntu-xenial:latest"

1. Submit the job and look at the output file. You should see output that indicates 
that the job was running on "Ubuntu" and has a newer version of GCC. 


