---
title: Smearing with eic-smear
name: eicsmear_1
category: eicsmear
layout: default
---

{% include layouts/title.md %}

* TOC
{:toc}

##### About #####

Currently hosted on
[github](https://github.com/eic/eic-smear)
and mirrored on [gitlab](https://gitlab.com/eic/eic-smear).
Detailed installation instructions are in the
[README.md](https://github.com/eic/eic-smear#building) file, but
for most users it is recommended to use centrally installed or packaged versions,
for example via
{% include navigation/pagelink.md folder=site.software name='eicsmear_generators_singularity' tag='cvmfs' %} or {% include navigation/pagelink.md folder=site.software name='escalate_singularity_1' tag='ESCalate' -%}.

The parameterizations are in a
[separate repository](https://github.com/eic/eicsmeardetectors)
to facilitate local installation and experimentation, but they too are of course
available centrally.

Contacts
* Alexander Kiselev <ayk@bnl.gov>
* Kolja Kauder <kkauder@bnl.gov>


##### Overview #####

Eic-smear is a Monte Carlo analysis package originally developed by the BNL EIC task force.
It is designed for use with the [ROOT](https://root.cern/) analysis framework.

It contains classes and routines for:
1. Building events in a C++ object and writing them to a ROOT file in a tree data structure.
2. Performing fast detector smearing on those Monte Carlo events.

Both portions of the code are included in the ```libeicsmear``` shared library.


##### Tree Generation #####
The tree-building portion processes plain text files, formatted according to
the EIC convention, into a ROOT TTree containing events.
A wide range of Monte Carlo generators are supported, including any that create HepMC2 or HepMC3 output.
Gzipped files, detected by the ```.gz``` extension, can be directly used as well.

Most of these are currently hosted on [GitLab](https://gitlab.com/eic/mceg).
Please see the associated documentation for further information on
individual generators.
Creation will typically be of the form
```bash
pythiaeRHIC < STEER_FILE > out.log
```
The output file name is specified in the steering card; the log file is needed
to save cross section information.
A few small example files are included for testing.

The basic command to generate a tree is
```
root [] gSystem->Load("libeicsmear");
root [] BuildTree ("pythia.txt.gz",".",-1, "log.txt");
Processed 10000 events containing 346937 particles in 6.20576 seconds (0.000620576 sec/event)
```

Each entry in the TTree named EicTree is a single C++ event object,
storing event-wise quantities common to all generators,
generator-specific variables and a list of particle objects.

##### Smearing #####
The smearing portion of the code provides a collection of classes and routines
that allow simple parameterizations of detector performance, provided via a
ROOT script, to be applied to any of the above Monte Carlo
events. Detector acceptance can also be defined. When run, a ROOT file with
smeared events is produced.

```
root [] gSystem->Load("libeicsmear")
root [] .L SmearMatrixDetector_0_1.cxx // Assuming you copied this here
root [] SmearTree(BuildMatrixDetector_0_1(),"pythia.root", "smeared.root",-1)
/-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-/
/  Commencing Smearing of 10000 events.
/-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-/
```

If you use the compiled collection ```libeicsmeardetectors```, a "nickname" functionality
can be used instead.

```
root [] gSystem->Load("libeicsmear")
root [] gSystem->Load("libeicsmeardetectors")
root [] auto det = BuildByName("matrix01");
root [] SmearTree( det,"pythia.root", "smeared.root",-1)
```

The output tree, called Smeared, will mirror the behavior of a true
detector system, i.e. it will only contain entries for particles that
were smeared (=measured), and only partial information if only parts
were smeared. E. g., if only momentum is smeared, the energy field will be
zero reflecting information gathered only by a tracker.
Particles further have methods of the form
```c++
bool psmeared = p->IsPSmeared();
```
to differentiate between "not measured" and "measured as 0".
In such a
case, the analyzer can of course rely on the truth level, but a more
realistic approach would be to make same kind of assumptions that one
would have to make for a physical detector, such as assuming pion mass
for all tracks baring PID information etc.

The smeared tree can be used by itself or linked to truth level information via
the ["friend" mechanism in ROOT](https://github.com/eic/eicsmeardetectors#analyze-the-tree).


For more details, examples, tests, please refer to the discussion in the [README](https://github.com/eic/eicsmeardetectors#basic-interactive-usage).

##### Available Detectors #####
The [detector repository](https://github.com/eic/eicsmeardetectors#official-parameterizations)
has a variety of paramterizations, some of them quite old and mostly preserved for legacy reasons, and
some of them semi-official variations of the official Yellow Report reference detector versions.
Here, we will only highlight three official versions.

##### Matrix 0.2 #####

Based on the [Detector Matrix](https://physdiv.jlab.org/DetectorMatrix) from November 21 2020.
**This is the most up-to-date version released by the YR DWG**
The implementation aims to be completely faithful (with perfect angular resolution) to the configuration available online.

Separate scripts are made for B=1.5T and B=3T, namely
```SmearMatrixDetector_0_2_B1_5T.cxx``` and ```SmearMatrixDetector_0_2_B3T.cxx```.
Shortcut names for ```BuildByName``` are  "MATRIXDETECTOR_0_2_B1_5T",
"MATRIX_0_2_B1_5T", and MATRIX02B15" (and similar for 3T).

###### Notes and details #####
* Where ranges are given, the more conservative number is chosen.

* Without available specifications, angular resolution is assumed to be perfect.

* Momentum and energy acceptance specifications are NOT implemented
  1. "90% acceptance" is not specific enough to introduce efficiency
  2. It is not a priori clear whether cuts should apply to truth or smeared value
  A "looper" may not reach the detector. On the other hand, a calorimetry deposit may fluctuate above threshold. Users should implement this acceptance as an afterburner. Alternatively, if a consensus can be achieved about the above, it can be added to the script as well.

* PID for pi/K/p is (crudely) implemented with perfect PID in the acceptance (and momentum range) where >3&sigma; separation is specified.

* Electron PID is not implemented. Very high purity is important and not handled well with the above approach


##### Matrix 0.1 #####

Based on the [Detector Matrix](https://physdiv.jlab.org/DetectorMatrix) from June 16.
This version was used for large parts of the first YR draft but should be replaced by 0.2 going forward.

There have been three changes with respect to the
{% include navigation/findlink.md name='eic_handbook_v1.2' title='Detector Requirements and R&D Handbook' -%}:

For the Backward Detector the Tracking Resolution column was updated as follows:
* eta =-3.5 - -2.5: σp/p ~ 0.1%×p+2.0% -> σp/p ~ 0.1%⊕0.5%
* eta=-2.5 - -2.0:  σp/p ~ 0.1%×p+1.0% -> σp/p ~ 0.1%⊕0.5%
* eta=-2.0 - -1.0:  σp/p ~ 0.1%×p+1.0% -> σp/p ~ 0.05%⊕0.5%

Notably, all assumptions made in the previous Handbook script have been removed
to be as faithful as possible to the matrix specifications.

The script name is ```SmearMatrixDetector_0_1.cxx``` and shortcut names for ```BuildByName```
are "MATRIXDETECTOR_0_1", "MATRIX_0_1", and "MATRIX01".

##### Handbook #####
Based on the initial table on page 26 of the {% include navigation/findlink.md name='eic_handbook_v1.2' title='Detector Requirements and R&D Handbook' -%}. Since the handbook at the time was versioned 1.2, the script inherited this version number,
but this does not indicate that it is "newer" than the matrix detectors.
**You should use Matrix 0.2 instead.**

The script name is ```SmearHandbook_1_2.cxx``` and shortcut names for ```BuildByName```
are "HANDBOOK_1_2" and "HANDBOOK".

###### Notes and details #####
Contrary to the matrix detctor scripts, I took some liberty where information was missing or incomplete:
* All angular resolutions are assumed to be 1 mrad. While this may be a good guess
for tracking, it is too optimistic for calorimetry.
* "Reasonable" constant terms were added to EMCal resolutions.
* A CMS-like barrel HCal was added.
* Forward and backward HCals with values from DWG discussions was used (45% stochastic, 6% constant).
