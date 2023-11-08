---
title: ESCalate
name: escalate_1
category: escalate
layout: default
toc: true
---

{% include layouts/title.md %}


#### Table of contents

- [About]()
- Fast HowTos:
   - [Run ESCalate]()  
   - [Run fast simulations]()
   - [Run full simulations]()   
- Detailed instructions:   
   - [Running in docker]()   
   - [Spack CVMFS central installation](#spack-cvmfs-central-installation)
   - [eic-image at jupyterhub.jlab.org]()
   - [Examples, tutorials and workspace]()
- [Remote work suggestions]()


# ESCalate framework


<img src="https://assets.gitlab-static.net/uploads/-/system/project/avatar/13083/logo-extra-whitespace.png" width="" style="float:left; width:20pt; padding: 3px;"/> [https://gitlab.com/eic/escalate](https://gitlab.com/eic/escalate)
<div style="clear: left;"></div>

> **ESC** - stands for **E**IC **S**oftware and **C**omputing group.
**ESC**alate combines together the software being develop by the group such as:

1. [Eic Smear](https://gitlab.com/eic/eic-smear.git) - fast simulation and rapid prototyping
2. [G4E](https://gitlab.com/eic/escalate/g4e.git) - a lightweight pure Geant4 full simulation of EIC detectors and beamlines
3. [Jana2](https://github.com/JeffersonLab/JANA2) - multi-threaded modular High Energy and Nuclear Physics event reconstruction framework
4. [eJana](https://gitlab.com/eic/escalate/ejana.git) - a layer around Jana2, that provides EIC related abstractions and leverages  reconstruction and analysis tools such as ACTS and Genfit for tracking, Cern ROOT, HepMC and others.
6. A bunch of other tools, tutorials and examples (
    [delphes](),
    [fastjet](),
    [pyjano](https://gitlab.com/eic/escalate/pyjano.git), [example plugins](https://gitlab.com/eic/escalate/plugins), [workspace](https://gitlab.com/eic/escalate/workspace)), validation data, etc.



[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://gitlab.com/eic/escalate/workspace)

<br>

## About ESCalate

The framework provides modularity in both ways: 
- package modularity: each package inside the framework can be used solely and separately from the rest of the framework
- subprocess modularity: jana (and ejana) modularize code in small libraries called plugins allowing to run tasks in a seamless multithreaded way. 

The Escalate itself ensures that data between packages is consistent and output of one package can be correctly read from another. It also provide python configuration to make it easy to organize a workflows between packages. Tools to install, maintain and deploy the subpackages


<br>

## Run Escalate

Try tutorials without installation on [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gl/eic%2Fescalate%2Fworkspace/master)

1. [Run using docker on your local machine](#running-in-docker),

2. [Using singularity (at labs or locally)](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_singularity.md)

3. [Run directly on Farms](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_ifarm.md)

4. [your browser in JLab jupyterlab]()  *(BNL jupyterhub is coming)*

### Fast simulations with simple command line

<img src="https://gitlab.com/eic/escalate/smear/-/raw/master/logo.png" width="150" style="float:left; padding: 10px 20px 10px 20px;"/>

**Fast simulations made simple**. It is easy to smear any EIC MCEG supported file with a single console command:

```bash
smear my_mc_file.txt
```
See the [documentation](https://gitlab.com/eic/escalate/smear/) for how to easily select a detector, its versions, and various other options

<div style="clear: left;"></div>

<br>

## Running in docker
*Instructions of how to install docker on Linux, Mac or Windonws and much more documentation on running ESCalate on docker [can be found on this page](https://gitlab.com/eic/containers/-/blob/master/README.rst)*

Running docker (JupyterLab):

```bash
docker run -it --rm -p8888:8888 electronioncollider/escalate:v1.1.0
```

After the docker runs, you can open JupyterLab environment in native web browser.
```sh
  http://127.0.0.1:8888/
```

You can bind any directory on your system to docker image by using **-v** flag:

```
-v <your/directory>:<docker/directory>
```

Convenient place inside docker image is

```
/home/eicuser/workspace/share
```


**(!) Important to know (!)** Each time `docker run` command is called, it spawns a new "container". A writeable layer where all modifications are saved is created for the 
 image, and then docker starts the container using the specified command. 
**A stopped container can be restarted** with all its previous changes intact using `docker start`

Please, read extended docker instructions on [docker.com](https://docs.docker.com/engine/reference/commandline/start/)


Troubleshooting and advanced used of docker [can be found on this page](https://gitlab.com/eic/containers/-/blob/master/README.rst)



<br>

## Spack CVMFS central installation

ESCalate framework is installed on CVMFS and can be used directly on farms with spack


**1. Source spack environemnt**

Tcsh/csh users (default for ifarm)

```
source /cvmfs/eic.opensciencegrid.org/packages/setup-env.csh
```

bash/zsh

```
source /cvmfs/eic.opensciencegrid.org/packages/setup-env.sh
```


**2. Load escalate module**

```
spack load escalate@1.1.0
```

It should be ready to work

**3. Test run**

```
which g4e
ejana
```



<br>



## eic-image at jupyterhub.jlab.org

Escalate framework image added to Jefferson Lab Jupyter Hub! It is still
in beta stage, several features are not yet working (and we are working on them). Someting works differently compared to when you run the image in docker. This page describe this. 


Go to {% include navigation/findlink.md name='jupyterhub.jlab.org' %} (follow authentication instructions if you are using it for the first time).


In the **Spawner options** -> **Select a notebook image**, 
set **eic-notebook (dev)** there

<img src="https://gitlab.com/eic/escalate/workspace/-/raw/master/img/jlab_jupyterhub_spawner.jpg" style="width:400pt">


You should end up in your jefferson lab home directory. 


<br>

### Differences with running in docker

When you run in docker you start as *eicuser* with user-ID=1000. On JupyterHUB you run in your JLab home directory with your CUE user (and your CUE user-ID). This implies that:

1. Your JLab home dir .bashrc is being called instead of one in docker. If you set custom python version or compiler in your .bashrc it will interfere with what is used in docker (will not work most of the time).

2. Since you start in your CUE home dir, you don't have the examples and tutorials. Just clone them to your JLab home dir:
    ```bash
    git lfs clone https://gitlab.com/eic/escalate/workspace.git   # 'lfs' is to pull data files!
    ```

3. All docker contents are readonly. One can't run ```sudo``` to elevate privilegies and change something in the container. Which means that you can't change eJana or G4E inside the docker, but you can install them in your home directory and [setup ejpm to use them](https://gitlab.com/eic/escalate/ejpm).


### What features are not available

1. Inspecting root files by ckicking on them. Solution - use uproot to explore the files. 

Please, share on slack if you have any further problems


<br>

## Examples, tutorials and workspace

When you run Docker on your machine or in cloud (such as Binder) the image is started with examples directory usually located at 

```
/home/eicuser/workspace
```

**Workspace** - is [a git repository](https://gitlab.com/eic/escalate/workspace) with collaborative workspace for EIC. It contains simulations including documentation, examples, and tutorials on how to get started with ESCalate framework.

When you start on farms, Singularity or Jupyterhub, it starts inside
your home directory and you may download (git clone) examples: 

```
git clone https://gitlab.com/eic/escalate/workspace
```


<br>

### Test run

Here are simple snippets with which you can test escalate working

Simpe API to smear a file

```
smear /path/to/file.txt

```

Here are 2 more advanced examples files for fast and full simulation. Please run them in your work directory. 

Fast simulation (eic-smear):

```python
from pyjano.jana import Jana

Jana().plugin('beagle_reader')\
      .plugin('open_charm')\
      .plugin('eic_smear', detector='jleic')\
      .plugin('jana', nevents=20000, output='hepmc_sm.root')\
      .source('/group/eic/mc/BEAGLE/eD_5x50_Q2_1_10_y_0.01_0.95_tau_7_noquench_kt=ptfrag=0.32_Shd1_ShdFac=1.32_Jpsidifflept_test40k_fixpf_crang.txt')\
      .run()
```

Full simulation:

```python
from g4epy import Geant4Eic
g4e = Geant4Eic()\
      .source('/group/eic/mc/BEAGLE/eD_5x50_Q2_1_10_y_0.01_0.95_tau_7_noquench_kt=ptfrag=0.32_Shd1_ShdFac=1.32_Jpsidifflept_test40k_fixpf_crang.txt')\
      .output('hello')\
      .beam_on(200)\
      .run()
```

Look at the docker or [tutorials repo](https://gitlab.com/eic/escalate/workspace) for more examples





