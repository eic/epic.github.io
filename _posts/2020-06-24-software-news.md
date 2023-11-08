---
title: Software News June
author: Markus Diefenthaler
layout: default
symbol: glyphicon-calendar
until: 2020-12-31
---
<p/>

{% include images/image.md name='news_banner' width='800' %}

The Software Working Group is working on physics and detector
simulations that enable a quantitative assessment of the measurement
capabilities of the EIC detector(s) and their physics impact for the
Yellow Report Initiative. The common simulation tools and workflow
environment being set up by the working group allows the EICUG to
pursue the Yellow Report studies in a manner that is accessible,
consistent, and reproducible.

## Table of contents

* [General Update](#swg)
    * [Communication](#communication)
    * [Detector Working Group: Detector Matrix Version 0.1](#dwg)
    * [GitHub for the EICUG](#github)
    * [Petrel: Worldwide data storage and sharing solution](#petrel)
    * [Support us to support you better](#support)
    * [Tutorials](#tutorials)
* [Software Update](#update)
    * [EicRoot](#eicroot)
    * [eic-smear](#eic-smear)
    * [ESCalate](#escalate)
    * [Fun4All](#fun4all)

---

## General Update {#swg}

### Communication {#communication}

The Software Working Group will start to announce software updates,
known bugs, and other software related news on the
[eicug-software@eicug.org](mailto:eicug-software@eicug.org) mailing
list. While summaries will be provided in our
Software News, we encourage all working groups to subscribe to
[eicug-software@eicug.org](mailto:eicug-software@eicug.org).



### Detector Working Group: Detector Matrix Version 0.1 {#dwg}

The Detector Working Group has frozen the current version of the
{% include navigation/findlink.md name='eic_handbook_v1.2' title='Detector Requirements and R&D Handbook' %}
as version 0.1. There have been three changes with respect to the
{% include navigation/findlink.md name='detector_matrix' title='Interactive Detector Matrix' %}:

For the Backward Detector the Tracking Resolution column was updated as follows:

* eta =-3.5 - -2.5: σp/p ~ 0.1%×p+2.0% -> σp/p ~ 0.1%⊕0.5%
* eta=-2.5 - -2.0:  σp/p ~ 0.1%×p+1.0% -> σp/p ~ 0.1%⊕0.5%
* eta=-2.0 - -1.0:  σp/p ~ 0.1%×p+1.0% -> σp/p ~ 0.05%⊕0.5% 

The abstract, reference files, and note sections of these fields have been updated accordingly. 

Parameterizations based on the Detector Matrix Version 0.1 are [available for usage](https://github.com/eic/eicsmeardetectors). You can select them via `SmearDetectorMatrix_0_1.cxx`. Please see [the section on Official Parameterizations](https://github.com/eic/eicsmeardetectors) for details. The parameterizations will be included in the QA plots macros of the upcoming next release of eic-smear. 

### GitHub for the EICUG {#github}

[https://github.com/eic](https://github.com/eic) is available for the whole EICUG. For membership, please send your GitHub username to [eicug-computing-infrastructure-support@eicug.org](mailto:eicug-computing-infrastructure-support@eicug.org?subject=GitHub%20Account)

We encourage you to share your software and documentation on [https://github.com/eic](https://github.com/eic). Please follow our guidelines in doing so: 

* Please use a unique name for your repository so that we can easily identify projects. We will, e.g., have various Geant4 related projects and each of these projects should have a name that is distinguishable from the others (and not Geant4). 
* Please [add at least one topic to your repository](https://help.github.com/en/enterprise/2.15/user/articles/classifying-your-repository-with-topics#adding-topics-to-your-repository). 
* Please give a clear description of your repository, e.g., ”Mary Johnson (Lab)’s DVCS MC” or “William's Smith (University) DIS analysis”. 



### Petrel: Worldwide data storage and sharing solution {#petrel}

We are happy to announce that we can offer the community
a large new data storage allocation on Petrel
to address the increasing need for an easy-to-access high-performance
storage solution for the Yellow Report Initiative.
Petrel is a Globus-enabled data service for researchers that provides a 
simple and intuitive interface for self-managed project-based data 
sharing. Our Petrel allocation is 100TB (more can be added if needed). 
The existing pre-grenerated Monte-Carlo data from BNL (13TB and growing)
are mirrored in the `BNL-gpfs02-data` directory, and available JLAB data
will be added soon as well. You can find more info on Petrel 
{% include navigation/findlink.md name='petrel' title='here'-%}.
Petrel is supported and maintained by ALCF.

Because of the Globus back-end it is easy to move, share and discover 
data via a single interface, regardless if you are working with on a HPC 
facility, computer farm or your local machine. All major laboratories 
and supercomputing facilities, as well as most universities support 
Globus. This allows you to use your existing laboratory (ANL, BNL, JLab, 
LBNL, etc.) or even university credentials to access the files. To 
access the storage space, [log in to globus.org](https://globus.org) 
with your existing laboratory or university credentials and access the 
`petrel#eic` endpoint. It's as easy as that!

Write-access is available to anyone in the EIC Globus Group.
If you want write-access to the storage space, 
or if you encounter any issues, 
you can contact [Sylvester Joosten](mailto:sjoosten@anl.gov) or 
[Markus Diefenthaler](mailto:mdiefent@jlab.org).

Additional storage options are available on [BNLBox/NextCloud](https://eic.github.io/computing/storage.html). 



### Support us to support you better {#support}
The common simulation tools and workflow environment can only grow
with user input. The EICUG Software Working Group asks interested
users to join our support team to ensure fast response to user
requests. This is an ideal opportunity to learn more about the
software and its development.



### Tutorials {#tutorials}
The tutorials for fast and detector full simulations are available on the [EICUG YouTube channel](https://www.youtube.com/channel/UCXc9WfDKdlLXoZMGrotkf7w). The next tutorials will be on Monte Carlo Event Generators. The dates will be slighlty adjusted due to the new dates for the EIC User Group Meeting (July 15-17).

<img src="{{ '/assets/images/tutorials/SWG-Tutorials.png' | relative_url }}" width="500"/>

---

## Software Update {#update}
In the June Software News, updates on EicRoot, eic_smear, ESCalate, and Fun4All are included. 

### EicRoot {#eicroot}

A new version of EicRoot has been prepared for the Yellow Report
initiative: The source code was cleaned up and all of the unnecessary
dependencies on third-party software packages were dropped. The
simulation framework now depends only on ROOT6, Geant4 and G3/G4 VMC
packages. ROOT5 support is discontinued.

The new version is available on
[https://github.com/eic/EicRoot](https://github.com/eic/EicRoot). Instructions
for how to install EicRoot locally or run a EicRoot Docker container
are provided.

EicRoot is used in the Yellow Report initiative for tracking studies
and the simulation of far-forward detectors and will be supported for
Yellow Report initiative and the work on the EIC CDR. The GDML (or
ROOT TGeo) export of pre-configured detector models into ESCalate of
Fun4All will be tested.

### eic-smear {#eic-smear}

##### Stand-alone detector repository with versioning

We have separated the detector smearing scripts from the main code. If you have eic-smear installed, use [this repository](https://github.com/eic/eicsmeardetectors) to quickly select and experiment with different versions simply via
```
$ git clone https://github.com/eic/eicsmeardetectors.git
$ root -l
root[] gSystem->Load("libeicsmear")
root[] .L eicsmeardetectors/SmearHandBook_1_2.cxx
root[] SmearTree(BuildHandBook_1_2(), "in.root", "out.root")
```
The [README.md](https://github.com/eic/eicsmeardetectors/blob/master/README.md), which nicely displays on the github page, has a table with all available versions and more instructions.
Improvements to eic-smear for comfortable compiling a local detector script and/or using a library of all available ones is in the works.


##### Advanced usage example

In order to demonstrate some of the more subtle issues of eic-smear output, we created an [example project]((https://github.com/eic/eicsmear-jetexample.git).

The repo just has two cxx files, meant to be self-documenting, and a detailed README. The first example demonstrates how to use just smeared output, the second how to compare to the truth level and/or how to extract some information from the truth level that should be in the smeared level but as of now isn't yet.

It is intentionally meant to be stand-alone, with a very simple Makefile and no cmake complications.
Please note that while the sample analysis is jet-specific, this is purely because that is a natural application to show 4-vector calculations from partial information.

---

### ESCalate {#escalate}

<img src="{{ '/assets/images/software/escalate/escalate-logo.png' | relative_url }}" width="150" style="float:left; padding: 10px 20px 10px 20px;"/>

**ESCalate** is a modern framework, which brings together Monte Carlo
  event generators, [eic-smear](https://gitlab.com/eic/eic-smear.git)
  for fast simulations, [G4E](https://gitlab.com/eic/escalate/g4e.git)
  for full detector simulations and
  [eJana](https://gitlab.com/eic/escalate/ejana.git) for analysis and
  reconstruction.  The framework is modular on package and library
  levels, provides command line interface, python and Jupyter APIs for
  user analysis and ensures data consistency between its loose coupled
  parts.

<div style="clear: left;"></div>

##### To run ESCalate: 

* [In docker on your machine (Linux, Mac, Win)](https://eic.gitlab.io/documents/quickstart/#ESCalate)
* [In singularity (on labs or locally)](https://eic.github.io/software/escalate_singularity_1.html)
* [JupyterHub at jupyterhab.jlab.org](https://gitlab.com/eic/escalate/workspace/-/blob/master/RemoteWork.md)
* Now also in Binder: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gl/eic%2Fescalate%2Fworkspace/master) **No installation needed** It might take some time to load - Binder is free, has limited resources, while our image is large because of ROOT, Geant4 and other packages. Even with the limited resources the link can be used as a live examples and demonstration.

##### MCEG

<img src="{{ '/assets/images/software/escalate/q2_jb.png' | relative_url }}" width="200" style="float:left; padding: 0px 20px 10px 20px;"/>

**MCEG examples**. [Examples](https://gitlab.com/eic/escalate/workspace/-/tree/master/01_fast_sim_tutorial) of using Pythia6 with radiative corrections and Pythia8+Dire to generate DIS events 
and process them with fast simulations added. With eJana you can instantly see the resulting DIS plots (see Validation section of eJana below)

<div style="clear: left;"></div>

**HepMC3 and file converter** As [HepMC3](https://gitlab.cern.ch/hepmc/HepMC3) was named "stable" with the 3.2.1 release, 
we switched eJana to work with HepMC3 (previously it worked with HepMC2), which can read files of both versions.

We also now provide ```hepmc_writer``` plugin. As eJana can read a number of Lund file formats (BeAGLE, HepEvents, PythiaEIC, etc.) it can be easily used as a converter of BeAGLE and other files to HepMC3 which later can be processed in [python](https://pypi.org/project/HepMC3/) or Delphes.

Example command to convert BeAGLE file to HepMC3:

```bash
ejana -Pplugins=beagle_reader,hepmc_writer beagle_file.txt
```

Beagle and other generators provide extended info such as true x, Q2
and other DIS values, one of the advantages of HepMC3 is that it
allows to have custom attributes for events and particles, i.e. allows
to save those values. At this point only basic values are converted,
but we are working to ship them all for BeAGLE and PythiaEIC.

##### Fast simulations 

<img src="https://gitlab.com/eic/escalate/smear/-/raw/master/logo.png" width="150" style="float:left; padding: 10px 20px 10px 20px;"/>

**Fast simulations made simple**. Now it is easy to smear any EIC MCEG supported file as simple as running a console command:

```bash
smear my_mc_file.txt
```
See the [documentation](https://gitlab.com/eic/escalate/smear/) for how to easily select a detector, its versions, and various other options

<div style="clear: left;"></div>

Smear tool can use different *smearing engines* under the hood. Eic-Smear C++ API is the main one, 
but one can also select detectors from Simple Smear written by Yulia Furletova. 
We are now considering integration of Delphes as the third engine.

The ```smear``` command requires installation of ROOT and other packages, so we have instructions for different
scenarios:

1. [Run docker on your local machine](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_docker.md),
2. [Using singularity (at labs or locally)](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_singularity.md)
3. [Run directly on IFarm or JLab Jupyterhub](https://gitlab.com/eic/escalate/smear/-/blob/master/simple_instruction_ifarm.md)
 
##### Full simulations

G4E has many changes related to forward and far forward region such as new ZDC, virtual tracking for B0 tracking, etc.. Results have been presented at the Pavia meeting and in the workshop on [Meson and Kaon Structure](https://indico.bnl.gov/event/8315/overview)).


<img src="{{ '/assets/images/software/escalate/G4E_with_zdc.png' | relative_url }}" height="250" style="float:left; padding: 10px 20px 10px 20px;"/>

<img src="{{ '/assets/images/software/escalate/g4e_lambda_decay_example.png' | relative_url }}" height="250" style="float:left; padding: 10px 20px 10px 20px;"/>

<div style="clear: left;"></div>

##### Reconstruction and analysis in eJana

**Standalone plugins**  You can always analyze the output of the ESCalate framework with ROOT macros, pyROOT or uproot. 
But to integrate your reconstruction algorithm, physics analysis or extend eJana, users can write their own plugins. 
Assuming that the most convenient workflow keep users' analysis in their own separate repositories, 
we added examples of standalone plugins, which work completely independently of eJana location, 
so no modification of the central repository is needed. 
The [standalone plugins examples are here](https://gitlab.com/eic/escalate/plugins) (and will be expanding).
 
It is also possible to use pyjano to generate and/or compile new plugins on fly. 
Newly generated plugins instantly work with command line, python or in Jupyter. 

**Validation and Analysis**. We added some validation and analysis plugins for ejana, that allows checking fast simulation
results as well as to build DIS plots (x, Q2, t, etc.) with a simple CLI command. 

Here is an example of how to read a Beagle file, build DIS plots with new ```dis``` plugin and write a ROOT tree, which you can analyze with ROOT 
macros or other tools.  

```bash
ejana -Pplugins=beagle_reader,dis,event_writer beagle_file.txt
```

**Reconstruction**. Recently, we have been overhauling our reconstruction part. At this moment we do track fitting through Genfit and vertexing through ACTS and working with ACTS developers on implementing full
ACTS tracking+vertexing so one can select and compare both. We also plan to use ACTS wider and put ACTS Fatras (Fast Tracking Simulation) to G4E. 

<img src="{{ '/assets/images/software/escalate/tracking_fit_crop.png' | relative_url }}" height="200" style="float:left; padding: 10px 20px 10px 20px;"/> 
We try to keep escalate packages easily installable on users machines ([using ejpm](https://gitlab.com/eic/escalate/ejpm)). ACTS requires C++17 and the latest Boost libraries and it might be a problem to build it even
on not too old machines without attaching CVMFS or using non system compilers. So we made ACTS a peer dependency - if one installs it, ejana is built with its reconstruction plugins. Without ACTS ejana it built with only fast simulation and minimal dependencies. 

<div style="clear: left;"></div>

##### Containers

**Containers versioning**. One can now get the [full versions table](https://gitlab.com/eic/containers) of all the software of ESCalate images and [the images change log](https://gitlab.com/eic/containers/-/blob/master/CHANGELOG.rst).
   
#### Help and support

[Subscribe to our Slack channel](https://join.slack.com/t/eicug/shared_invite/zt-djjvylq9-zmRphsvRLpDBiYb_jJwgCQ) to get help with the software and participate in other channels like Yellow Report analysis. 

---

### Fun4All {#fun4all}
Fun4All is a well established simulation/reconstruction framework initially developed to reconstruct and analyze data from the PHENIX experiment. In recent years it has provided the simulations to shepherd the sPHENIX experiment from the first drawing on a napkin all the way through CD1. For the EIC it was used to develop a concept for an [EIC detector based on the Babar magnet](https://arxiv.org/pdf/1402.1209.pdf) and [an EIC detector Built around the sPHENIX Solenoid](https://indico.bnl.gov/event/5283/attachments/20546/27556/eic-sphenix-dds-final-2018-10-30.pdf). It can be used interactively (run a few events, have a look, run a few more, look again) as well as running massive productions on large farms (and as of recent also on CORI and on the OSG). For more details (and program flow charts) have a look at the [presentation](https://indico.in2p3.fr/event/18281/contributions/71607/) at the 2019 EIC User Group Meeting.

##### New developments
All fieldmaps (Babar, Beast and JLeic) are now available. They can be chosen on the ROOT macro level for details have a look at this [tutorial](https://github.com/eic/fun4all_eic_tutorials/tree/master/MagneticField).

<img src="{{ '/assets/images/software/fun4all/fun4all_fieldmaps.png' | relative_url }}" width="800"/>

The magnetic field was verified using charged geantinos (shown here are 0.5, 1, 2, 3 GeV/c) compared to constant solenoidal fields as shown on the right for the Beast magnet.

Importing detectors via gdml files has its quirks but is now well understood and fully supported. It is not possible to do this generically since one has to address the volumes by name but this only needs minor modifications to existing code and is largely cut and paste.

<img src="{{ '/assets/images/software/fun4all/fun4all_gdml.png' | relative_url }}" width="800"/>

Here are the latest additions to our selection of implemented detectors (all of which can be combined in a ROOT macro). We have a detailed model of the [Beast magnet](https://github.com/eic/fun4all_eicdetectors/tree/master/source) (already used in the fieldmap plots) as well as the [*All Silicon Tracker*](https://github.com/eic/g4lblvtx) and the EIC beam pipe.

The ability to combine these (and other detectors) at will is shown here:

<img src="{{ '/assets/images/software/fun4all/fun4all_allsilicon.png' | relative_url }}" width="800"/>

where the *All Silicon Tracker* is put inside the Beast magnet and is being used as central tracker inside the EIC detector based on the Babar magnet.


##### Verification
Fun4All is also being used to reconstruct test beam data which allows a direct comparison of simulations with data. sPHENIX has performed extensive test beam campaigns for its detectors at energies which are relevant for EIC detectors. The calorimeter test beam results have been [published](https://ieeexplore.ieee.org/document/8519782) recently, the analysis of TPC and Maps detector data is ongoing. A good agreement with previous EICRoot based simulations has been found for the All Silicon Tracker ([slide 8](https://indico.bnl.gov/event/7894/contributions/37609/attachments/28098/43125/200514_AllSi_in_Fun4All_2.pdf))

##### Help with Fun4All
Help is available via [our support channel in Mattermost](https://chat.sdcc.bnl.gov/eic/channels/fun4all-software-support), non Scientific Data Center Accounts need an invite - contact us. 

---

### About the Software Working Group

Please see our [website](https://eic.github.io) for more information
about the Software Working Group and engage in the discussion on our
[mailing list](mailto:eicug-software@eicug.org). For software
questions, please see our
[tutorials](https://www.youtube.com/channel/UCXc9WfDKdlLXoZMGrotkf7w),
contact us via
[software-support@eicug.org](mailto:software-support@eicug.org), or
join our [Slack channel](https://eicug.slack.com) (see QR code below).

<img src="{{ '/assets/images/support/EICUG-Slack.png' | relative_url }}" width="200"/>

--- 
