# MACH - Fly Faster
```
    __  ______   ________  __
   /  |/  /   | / ____/ / / /
  / /|_/ / /| |/ /   / /_/ /
 / /  / / ___ / /___/ __  /
/_/  /_/_/  |_\____/_/ /_/
=============================
```
A bash wrapper for fly to allow faster access to standard fly commands. Got sick of using long commands and trying to remember the right tags for things all the time while doing development work with fly and concourse.

It is used in the directory that contains your pipeline.

It is not designed as a replacement for all fly commands, just ones that are more commonly used in development phases.

## Requirements
Fly must be installed in your PATH somewhere too. Download fly from [Concourse Downloads](https://concourse.ci/downloads.html)

## Installation
Put `mach` in your PATH somewhere, `~/bin` is a good spot if it is in your PATH.

## Usage
List of commands that are supported:
### Pipelines
* `sp` set pipeline, will automatically create a pipeline based on the current working directory name, override with `-p <pipeline>` or specify in `.mach` file
* `up` unpause pipeline
* `dp` destroy pipeline
* `pp` pause pipeline
* `spup` set pipeline and unpause it

### Jobs
* `tj` trigger job, will present a list of jobs based on the pipeline configuration file and allow user to select, and automatically uses the pipeline based on the current working directory name, override with `-p <pipeline> -j <jobname>`
* `w` watch job
* `tjw` trigger job then watch it
* `pj` pause job
* `uj` unpause job

### Resources
* `cr` check resource, will perform a check on a resource from a list based on the pipeline config, uses pipeline based on the current working directory, override with `-p <pipeline> -r <resourcename>`
* `pr` pause resource
* `ur` unpause resource

## Using MACH
Run using `mach <option>`

### Example usage
Change into a directory that contains a pipeline you want to set up, then run `mach sp`. MACH will check that the directory contains a pipeline and vars file, then it will pass off to fly using something like `fly -t ${FLYTARGET} sp -c ${PIPEFILE} -l ${VARSFILE} -p $(basename ${PWD})`

### Set up
Create a `.mach` file in the directory with your pipeline and vars
Use variables to define your MACH environment

Uses the `.mach` file to allow access to fly easily with shortcuts using preconfigured settings.

Populate your `.mach` file with your fly target by entering `FLYTARGET=targetname`

### .mach file
Defaults to looking for `ci/pipeline.yml` and `ci/vars.yml` unless overrides using exports are in place

* `FLYTARGET=example` sets your fly target
* `PIPEFILE=ci/pipeline.yml` sets the pipeline file to use
* `VARSFILE=ci/vars.yml` sets the vars file if you have one
* `PIPELINE=pipeline` sets the name of your pipeline if it differs from the base directory name

MACH will source this file when executed from within the directory it is contained, and use the options defined inside.
