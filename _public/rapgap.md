---
title: RAPGAP
name: rapgap
category: rapgap
layout: default
---

{% include layouts/title.md %}

* TOC
{:toc}


#### About

RAPGAP is a Monte Carlo generator which can be used to generate both DIS and Diffractive e+p events.

Currently hosted on [GitLab](https://gitlab.com/eic/mceg/RAPGAP-3.302)

Contact (for this version)
* Kolja Kauder <kkauder@bnl.gov>


#### Available Documentation

* Manual: [rapgap.pdf](http://projects.hepforge.org/rapgap/rapgap.pdf)
* www-page: [http://projects.hepforge.org/rapgap/]
* RAPGAP has radiative corrections included. The program used is HERACLES. Info can be found [here](http://www.desy.de/~hspiesb/heracles.html)
* [Here](https://wiki.bnl.gov/eic/index.php/RAPGAP_compile) are some notes how to compile rapgap-32 for 32-bit and 64bit on the racf-cluster.
* <span style="color:red">Warning:</span> Prior to version 3.1, Rapgap had a known bug; to calculate t from the incoming and outgoing proton did not work. However, the current version is 3.3.

#### Running RAPGAP

An installation of RAPGAP can be found in the EIC cmvfs region at
```sh
$EICDIRECTORY/PACKAGES/RAPGAP-3.302
```

When you run RAPGAP, you will create large text files as output.
Therefore, you should do so from an appropriate DATA directory.
Inside this directory, you should create a soft link to the rapgap executable code:
```sh
ln -s $EICDIRECTORY/bin/rapgap33
```

Now, you need to copy a steer file from the RAPGAP directory to your own directory,
so you can edit it and run what you wish. For example:
```sh
cp $EICDIRECTORY/PACKAGES/RAPGAP-3.302/data/steer-ep steer
```

Once this is done, you are ready to run. Once you have edited the file, then you run with the command:
```sh
./rapgap33 < steer > rapgap.log
```

This creates a text file named `rapgap.txt` with the raw rapgap output. At this point, you are ready to create a ROOT tree and analyze your data.


#### Output file structure
The ascii output, `rapgap.txt`, has the following structure:

* 1st line: RAPGAP EVENT FILE
* 2nd line: "============================================"
* 3rd line: Information on event wise variables stored in the file

| I:                                                                                                            | 0 \(line index\)                                                                                            |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| ievent:                                                                                                       | eventnumber running from 1 to XXX                                                                           |
| genevent:                                                                                                     | trials to generate this event                                                                               |
| subprocess:                                                                                                   | generated subprocess, for details see page in the rapgap\-manual                                            |
| idir:                                                                                                         | select type of events to be generated                                                                       |
| | = 1 standard inelastic scattering                                                                             |
| | = 0 diffractive and pion exchange processes                                                                   |
| idisdif:                                                                                                      | mixing of standard inelastic scattering, diffractive and pion exchange processes according to cross section |
| | 0 generates only the processes selected by IDIR\.                                                             |
| | 1 mixing of standard inelastic and diffractive processes\.                                                    |
| | 2 mixing of standard inelastic, diffractive and pion exchange processes                                       |
| cross section:                                                                                                | integrated cross section                                                                                    |
| error on cross section:                                                                                       | standard deviation of integrated cross section                                                              |
| s:                                                                                                            | total center of mass energy                                                                                 |
| Q2, y:                                                                                                        | in lepto\-production: actual Q2 of γ; energy fraction lost by incident electron                             |
| | If radiative corrections are turned on they are different from what is calculated from the scattered lepton\. |
| | If radiative corrections are turned off they are the same as what is calculated from the scattered lepton     |
| xgam:                                                                                                         | energy fraction of parton on electron side                                                                  |
| xpr:                                                                                                          | energy fraction of parton on proton side                                                                    |
| pt\_hat:                                                                                                      | phat\_⊥ \[GeV/c\] of parton in hard subprocess cm system                                                    |
| pt2\_hat:                                                                                                     | phat^2\_⊥ \[GeV2/c2\] of parton in hard subprocess cm system                                                |
| s\_hat:                                                                                                       | invariant mass ˆs \[GeV2\] of hard subprocess                                                               |
| t\_hat:                                                                                                       | for diffractive processes T2GKI = t \[GeV2\]                                                                |
| x\_pom:                                                                                                       | for diffractive processes XFGKI = xIP                                                                       |
| s\_hat:                                                                                                       | shat of hard subprocess                                                                                     |
| z:                                                                                                            | z = p\_i\*p\_f/p\_i\*q = ZQGKI                                                                              |
| x:                                                                                                            | xp = Q2/2p\_i\*q = XPGKI                                                                                    |
| phi:                                                                                                          | φ = PHIGKI azimuthal angle                                                                                  |
| nrTracks:                                                                                                     | number of tracks in this event, includes also virtual particles                                             |
{:.table-bordered}
{:.table-striped}
<br />

* 4th line: "============================================"
* 5th line: Information on track wise variables stored in the file

| I:      | line index, runs from 1 to number of particles                                                           |
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

* 6th line: "============================================"
* 7th line: event information for first event
* 8th line: "============================================"
* 9th to X-1 line: track-wise info of 1st event
* Xth line "=============== Event finished ==============="

**The information from line 7 to X-1 repeats for each event.**

#### How to analyze events
Create a ROOT tree using the [eic-smear](eicsmear.html#tree-generation) package.

#### Additional Information

*  We recommend against using version 3.303. The changes made to support LHAPDF6
 seem to make using LHAPDF5 impossible

* A number of completed RAPGAP data files for various e+p energies are stored on RCF
and can be [made available](../resources/storage.html) upon request.

<br />
<br />
<br />
