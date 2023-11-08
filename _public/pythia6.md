---
title: PYTHIA6 with Radiative Corrections
name: pythia6
category: pythia
layout: default
---

{% include layouts/title.md %}

* TOC
{:toc}


#### About

Based on PYTHIA 6.4.28 with radiative corrections and output
modifications to support [eic-smear](eicsmear.html).

Currently hosted on [GitLab](https://gitlab.com/eic/mceg/PYTHIA-RAD-CORR)

Contacts
* Kolja Kauder <kkauder@bnl.gov>
* Elke Aschenauer <elke@bnl.gov>


#### Overview of PYTHIA

* For e-p collisions, only versions of PYTHIA 6.4 can be used, the
  absolute latest version 6.4.28 is part of this package and all improvements are integrated.
* Over the years we have implemented many changes to PYTHIA to correct some problems we found and to integrate some e-A physics. All is documented [here](#PYTHIAChanges).
Pythia8 does not yet fully support e-p collisions.
* Note the [webpage](http://pythia6.hepforge.org), the
[manual](http://www.thep.lu.se/~torbjorn/pythia/lutp0613man2.pdf),
and the [notes on updates](http://www.hepforge.org/archive/pythia6/update_notes-6.4.28.txt) after the manual got published.
* A file containing all particle codes and decay channels with branching ratios implemented in pythia:  [PYTHIA PDG](http://www.phenix.bnl.gov/WWW/publish/elke/EIC/Files-for-Wiki/Pythia.6.4.particles.codes.and.decays.txt).

##### Pythia processes important in e-p #####


| Subprocess                 | \#                                  | Description |
|----------------------------|-------------------------------------|-------------|
| **SOFT VMD**||
| V N → V N                           | 91  | elastic VMD                       |
| V N → X N                           | 92  | single\-diffractive VMD           |
| V N → V X                           | 93  | single\-diffractive VMD           |
| V N → X X                           | 94  | double\-diffractive VMD           |
| V N → X                             | 95  | soft non\-diffractive VMD low\-pT |
| **QCD 2→2**                        |     |                                   |
|                                     | 96  | semihard QCD 2→2                  |
| **RESOLVED \(hard VMD and anomalous\)** |     |                                   |
| qq → qq                             | 11  | QCD 2 → 2\(q\)                    |
| q qbar → q qbar                     | 12  |                                   |
| q qbar → gg                         | 13  |                                   |
| gq → gq                             | 28  |                                   |
| qg → qg                             | 28  | QCD 2 → 2\(g\)                    |
| gg → q qbar                         | 53  |                                   |
| gg → gg                             | 68  |                                   |
| **DIRECT**                        |     |                                   |
| γ<sup>*</sup>q → q                             | 99  | LO DIS                            |
| γ<sup>*</sup>T q → qg                          | 131 | \(transverse\) QCDC               |
| γ<sup>*</sup>L q → qg                          | 132 | \(longitudinal\) QCDC             |
| γ<sup>*</sup>T g → q qbar                      | 135 | \(transverse\) PGF                |
| γ<sup>*</sup>L g → q qbar                      | 136 | \(longitudinal\) PGF              |
{:.table-bordered}
{:.table-striped}

<br />
**VMD** Vector Meson Dominance, describing the elastic diffractive
production of Vector Mesons.
<br />
**QCDC**: QCD-Compton, radiation of a gluon
from incoming or outgoing quark lines.
<br />
**PGF**: Photon Gluon Fusion.
<br />

#### Running pythiaeRHIC

In the standard setup in the singularity cvmfs environment or at BNL, the code, executable, steering files,
plus shared libraries for PYTHIA and RADGEN, can be found in
```
$EICDIRECTORY/PACKAGES/PYTHIA-RAD-CORR
$EICDIRECTORY/lib
$EICDIRECTORY/bin
```

The executable `pythiaeRHIC` (in the future most likely renamed to pythia-eic) is the program for running PYTHIA.
It is based on PYTHIA 6.4.28 and was modified to include radiative corrections using [RADGEN](#radiative-corrections).

To run, the executable reads options from standard input
and is controlled via steering cards and input redirection, with
optional output redirection to a log file. The name of the output file
is specified in the steering card.
```
pythiaeRHIC < STEER_FILE > out.log
```

A practical example, assuming $EICDIRECTORY was set and the package
installed as above, is:
```
$EICDIRECTORY/bin/pythiaeRHIC < $EICDIRECTORY/PACKAGES/PYTHIA-RAD-CORR/STEER-FILES-Other/ep_noradcor.20x250.quicktest
```

The output ASCII format is described [below](#ASCII output file structure).


<!--Output can be produced in a ROOT tree format, described [here](eicsmear.html), or an ASCII file, described [[#ASCII output file structure | below]].
See the included README for further instructions on building and running.-->


##### Using Radiative Corrections

* Create a directory called radgen in the area you want to run the code.
* Either generate the lookup table for your cuts and beam energy settings first...
```
pythiaeRHIC < input.data_make-radcor.eic
```
* ... or use one of the files already generated in
```
$EICDIRECTORY/PACKAGES/PYTHIA-RAD-CORR/radgen
```
Files are named "radgen(electron beam energy)x(proton beam energy)", for example "radgen10x100" or "radgen4x100".
* To run the code with radiative corrections,  simply change the steer file to either input.data_radcor.eic or input.data_radcor.VM.eic and type
 pythiaeRHIC < input.data_radcor.eic > XXX.log

* The above are example steering files. The main difference to running without radiative corrections is the switch in line 9 of a steering file:
```
switch for rad corrections !* 0: no, 1: yes, 2: generate lookup table
```
However, settings for x and y have to be appropriate. The following should work in most cases:
```
1e-05,0.99        ! xmin and xmax
5e-03,0.99        ! ymin and ymax
```

##### Steering files
A variety of steering files can be found in the `STEER-*`
directories. Which ones appropriately reflect the physics you wish to
implement is beyond the scope of this document. Please contact the EIC
UG for advice.




##### Executables
* `pyMaineRHIC_v2.f` is the main program used to generate `pythiaeRHICe`
* `pythiaMain.cpp` and `UsingCardPythiaMain.cpp` are C++ implementations using either the class interface functions or the same steering file as the fortran driver. They are not currently producing standard ASCII output however and are meant as demonstrators only. The reason for this is that output generation is hard-coded into multiple places inside the pythia and radgen fortran files.


<!--
===Original version===

The original version of the code supported ASCII output only, but is the same in physics content (e.g. supporting radiative corrections).
An installation is located at
 /afs/rhic.bnl.gov/eic/PACKAGES/PYTHIA-RAD-CORR-32BIT
This directory contains several example steer files, named "input.data. XXXXX.eic", which also work with the new code.
There are also generated [[#2) with radiative corrections | RADGEN lookup tables]].
Here are example files for [[Media:Input.data.eAu_noradcorr.eic.pdf‎ | eAu]] and [[Media:Input.data.ep_noradcorr.eic.pdf‎ | ep]].
The main program, named pyMaineRHIC.f, can be viewed [[Media:PyMaineRHIC-Jan2012.pdf | here]].
Several other routines are needed, which are in the same directory.
-->

##### Output file structure

The output file is in a text format which has the following structure:
* 1st line: <tt>PYTHIA EVENT FILE</tt>
* 2nd line: <tt>============================================</tt>
* 3rd line: Information on event wise variables stored in the file:

| I:                                    | 0 (line index)                                                                                                                                                               |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ievent:                               | eventnumber running from 1 to XXX                                                                                                                                            |
| genevent:                             | trials to generate this event                                                                                                                                                |
| subprocess:                           | pythia subprocess (MSTI(1)), for details see table above                                                                                                                     |
| nucleon:                              | hadron beam type (MSTI(12))                                                                                                                                                  |
| targetparton:                         | parton hit in the target (MSTI(16))                                                                                                                                          |
| xtargparton:                          | x of target parton (PARI(34))                                                                                                                                                |
| beamparton:                           | in case of resolved photon processes and soft VMD the virtual photon has a hadronic structure. This gives the info which parton interacted with the target parton (MSTI(15)) |
| xbeamparton:                          | x of beam parton (PARI(33))                                                                                                                                                  |
| thetabeamparton:                      | theta of beam parton (PARI(53))                                                                                                                                              |
| truey, trueQ2, truex, trueW2, trueNu: | are the kinematic variables of the event.                                                                                                                                    |
|                                       | If radiative corrections are turned on they are different from what is calculated from the scattered lepton.                                                                 |
|                                       | If radiative corrections are turned off they are the same as what is calculated from the scattered lepton                                                                    |
| leptonphi:                            | phi of the lepton (VINT(313))                                                                                                                                                |
| s\_hat:                               | shat of the process (PARI(14))                                                                                                                                               |
| t\_hat:                               | Mandelstam t (PARI(15))                                                                                                                                                      |
| u\_hat:                               | Mandelstm u (PARI(16))                                                                                                                                                       |
| pt2\_hat:                             | pthat^2 of the hard scattering (PARI(18))                                                                                                                                    |
| Q2\_hat:                              | Q2hat of the hard scattering (PARI(22)),                                                                                                                                     |
| F2, F1, R, sigma\_rad, SigRadCor:     | information used and needed in the radiative correction code                                                                                                                 |
| EBrems:                               | energy of the radiative photon in the nuclear rest frame                                                                                                                     |
| photonflux:                           | flux factor from PYTHIA (VINT(319))                                                                                                                                          |
| nrTracks:                             | number of tracks in this event, includes also virtual particles                                                                                                              |
{:.table-bordered}
{:.table-striped}

* 4th line: <tt>============================================</tt>
* 5th line: Information on track-wise variables stored in the file:

| I:      | line index, runs from 1 to nrTracks                                                                      |
| ------- | -------------------------------------------------------------------------------------------------------- |
| K(I,1): | status code KS (1: stable particles 11: particles which decay 55; radiative photon)                      |
| K(I,2): | particle KF code (211: pion, 2112:n, ....)                                                               |
| K(I,3): | line number of parent particle                                                                           |
| K(I,4): | normally the line number of the first daughter; it is 0 for an undecayed particle or unfragmented parton |
| K(I,5): | normally the line number of the last daughter; it is 0 for an undecayed particle or unfragmented parton. |
| P(I,1): | px of particle                                                                                           |
| P(I,2): | py of particle                                                                                           |
| P(I,3): | pz of particle                                                                                           |
| P(I,4): | Energy of particle                                                                                       |
| P(I,5): | mass of particle                                                                                         |
| V(I,1): | x vertex information                                                                                     |
| V(I,2): | y vertex information                                                                                     |
| V(I,3): | z vertex information                                                                             |
{:.table-bordered}
{:.table-striped}
<br />

* 6th line: <tt>============================================</tt>
* 7th line: event information for first event
* 8th line: <tt>============================================</tt>
* 9th to X-1 line: track-wise info of 1st event
* Xth line <tt>============================================</tt>

**For each subsequent event, lines 7 through X repeat analogously.**

##### How to analyze events

The recommended way is to create and use a ROOT tree with the `BuildTree` function and other tools provided by [eic-smear](eicsmear.html).
Some guidelines regarding Monte Carlo normalization can be found [here](https://wiki.bnl.gov/eic/index.php/Simulations#MC_Analysis_Techniques).

##### TPythia6
ROOT supports an interface to PYTHIA via the class TPythia6. This allows PYTHIA to be configured, run and analysed from within a ROOT session, which can be a more convenient than using PYTHIA as a separate programme. The EIC installations of ROOT support TPythia6, but be aware that the version of PYTHIA interfaced with ROOT is the "vanilla" distribution, and hence lacks the EIC additions. If you require these for your analysis you should only use the standalone pythiaeRHIC program.
This means you miss:
* the possibility for radiative corrections
* a correct treatment of the pt of the remnant
* all the improvements to the VMD model in PYTHIA to describe the HERA exclusive VM data
* the possibility to run with nuclear PDFs

<br />
<br />
<br />


#### Installation

It is recommended to take advantage of the pre-installed versions on the lab farms or
the available stand-alone [singularity](eicsmear_generators_singularity.html) or [escalate](escalate_singularity_1.html) containers.

However, the package can also be built using cmake and make.


##### Prerequisites
- gfortran
- LHAPDF5
- ROOT
- cmake
- CERNLIB

##### Basic installation

(Adapt directories etc as appropriate.)

```
cd ${EICDIRECTORY}/PACKAGES
git clone https://gitlab.com/eic/mceg/PYTHIA-RAD-CORR.git
cd ${EICDIRECTORY}/PACKAGES/PYTHIA-RAD-CORR
mkdir build; cd build
cmake ../ -DCMAKE_INSTALL_PREFIX=${EICDIRECTORY}
make -j 8 install
```
This will produce warnings of the form
```
Warning: $ should be the last specifier in format at (1)
```
which should be okay (it is a g77 extension allowed by gfortran).

Executables require LHAPDF5 installed and links executables against it.


##### CHANGES with respect to the build_pythia6.sh script

* LHAPDF-related:
  * pdfset.f structm.f structp.f are removed (renamed)
  * sugra.f is emptied out

* Fix for older pythia versions:
  * pyalps.f doesn't need to be changed in 6.4.28
   (build script only affects commented out old code)

* Random number generation change:
  * Replace default routine by call to ranlux
  * pyr.f
  * Added ranlux.f v 1.2 (Deleted an include line) to avoid issues linking
  against cernlib

* PHYSICS:
  * pydiff.f
  * pydisg.f
  * pygaga.f
  * pyrand.f
  * pyremn.f
  * pysgqc.f
  * pysigh.f
  * pyxtot.f
  * pydata.f
  * pyptdirc.f

* Added in top directory:
  * radgen.f
  * radgen_event.f
  * radgen_init.f
  * gmc_random.f
  * pyth_xsec.f
  * pythia_radgen_extras.f


#### Radiative Corrections ####

The code implemented in PYTHIA to calculate radiative corrections is called RADGEN.
The writeup on it can be found [here](http://arXiv.org/pdf/hep-ph/9906408).
The following steps have been done to implement it in PYTHIA:
* change the subroutine pygaga.f so it calls RADGEN after you have thrown y and Q2
* get the true y and Q2 from RADGEN and the radiated photon
* Pythia will continue to now generate an event based on this y and Q2
* Pythia still operates under accept reject, the extra weight from the radiative corrections is absorbed in the flux factor

##### Additional Info on radiative corrections #####
Webpages with codes:
* [http://www.jlab.org/RC](http://www.jlab.org/RC)
* [http://www.desy.de/~heramc/mclist.html](http://www.desy.de/~heramc/mclist.html)
* Nuclear beams in HERA.

Other references:
M. Arneodo, (Turin U. & INFN, Cosenza) , A. Bialas, (Jagiellonian U.) , M.W. Krasny, (Paris U., VI-VII & Paris U., VI-VII) , T. Sloan, (Lancaster U.) , M. Strikman, (Penn State U.) . Sep 1996. 40pp.
To be published in the proceedings of Workshop on Future Physics at HERA,
Hamburg, Germany, 25-26 Sep 1995.
In *Hamburg 1995/96, Future physics at HERA* 887-926.
e-Print: hep-ph/9610423
and the references 5 and 6 in this article.
To find them check [here](http://www.desy.de/~heraws96/).
