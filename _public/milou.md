---
title: MILOU
name: milou
category: milou
layout: default
---

{% include layouts/title.md %}

* TOC
{:toc}


#### About

MILOU is a Monte Carlo generator for deeply virtual Compton scattering (DVCS),
ep → eYγ, developed by E. Perez, L. Schoeffel and L. Favart[^1]. It is based on
generalized parton distributions (GPDs) evolved to next-to-leading order.


Currently hosted on [GitLab](https://gitlab.com/eic/mceg/milou)

Contact (for this version)
* Kolja Kauder <kkauder@bnl.gov>


#### Overview

The MILOU code is written in Fortran. GPDs, evolved to next-to-leading order,
provide the real and imaginary parts of Compton form factors (CFFs),
which are used to calculate cross sections for DVCS and DVCS-BH interference.
The package BASES/SPRING[^2] is used to generate events from these cross sections.
First, the differential cross sections are integrated by the numerical integration
package BASES to yield probability distributions.
These distributions are used by the event generation package SPRING to generate
the DVCS events. Proton dissociation (ep → eYγ) can be included, with hadronization
of the system Y performed by [PYTHIA](pythia6.html).

#### Running MILOU

A 32-bit installation of MILOU can be found in the EIC cmvfs region at
```sh
$EICDIRECTORY/PACKAGES/milou
```

Note: At JLab, the 32-bit fortran libraries are unavailable. The
simplest solution is to use
[singularity](eicsmear_generators_singularity.md):
```sh
module load singularity
export EIC_LEVEL=dev
source /cvmfs/eic.opensciencegrid.org/x8664_sl7/MCEG/releases/etc/eic_bash.sh
```

The generator options are set via a "steering card" dvcs.steer. The options are described in the manual[^1].

Note that the executables expect the .dat files and the dvcs.steer file
in the directory of execution. Easiest way to achieve this is a softlink (adapt to your location)
```sh
cp $EICDIRECTORY/PACKAGES/milou/dvcs.steer .
ln -s $EICDIRECTORY/PACKAGES/milou/*.dat .
```

You can then run with:
```sh
$EICDIRECTORY/bin/milou
```

The generated events are saved to a PAW ntuple, as well as to an ascii file compatible
with [eicsmear](eicsmear.html). The output file names are hard-coded to
`15x50_dvcs.ntp` and `asc_15x50_dvcs.out`, respectively.

The program `h2root` can be used to produce a [ROOT](https://root.cern.ch/) ntuple from the PAW ntuple:

```sh
h2root 15x50_dvcs.ntp <rootFileName>
```

#### Output file structure
The ascii output, `asc_15x50_dvcs.out`, has the following structure:

* 1st line: "generator name" (i.e. MILOU32); "name of the person generating the sample"; "Name of the Istitution".
Currently hard-coded to ```"MILOU32     S. Fazio        BNL"```
* 2nd line: MILOU EVENT FILE
* 3rd line: "============================================"
* 4th line: Information on event wise variables stored in the file

| I:                                                                                                            | 0 \(line index\)                                                                                                    |
|---------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| ievent:                                                                                                       | eventnumber running from 1 to XXX                                                                                   |
| linesnum:                                                                                                     | numbers of particles in the event \(max value of line index\); =5 if no radiative corrections applied, =6 otherwise |
| weight:                                                                                                       | applied weight, default is 1\.00000000                                                                              |
| genprocess:                                                                                                   | generated process \(1=BH, 2=DVCS, 3=Interaction\(btw BH and DVCS\), 4=BH\+DVCS\+Interaction, 5=SSA without TW3\)    |
| radcorr:                                                                                                      | radiative corrections \(0= NO correction; 1= Initial State Radiation\(ISR\) \)                                      |
| truex, trueQ2, truey, truet, truephi:                                                                         | are the kinematic variables of the event\.                                                                          |
| phibelgen:                                                                                                    | azimuthal angle between the production and the scattering plane\.                                                   |
| phibelres:                                                                                                    | azimuthal angle \(see above\) resolution\.                                                                          |
| phibelrec:                                                                                                    | reconstructedazimuthal angle between the production and the scattering plane\.                                      |
| | If radiative corrections are turned on they are different from what is calculated from the scattered lepton\. |
| | If radiative corrections are turned off they are the same as what is calculated from the scattered lepton     |
{:.table-bordered}
{:.table-striped}
<br />

* 5th line: "============================================"
* 6th line: Information on track wise variables stored in the file

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

* 7th line: "============================================"
* 8th line: event information for first event
* 9th line: "============================================"
* 10th to X-1 line: track-wise info of 1st event
* Xth line "=============== Event finished ==============="

**The information from line 8 to X-1 repeats for each event.**

#### Known Issues
Cannot currently be built with 64 bits. This is most likely fixable (it does compile, but the output contains NaNs).

#### References
[^1]: "MILOU: a Monte-Carlo for Deeply Virtual Compton Scattering", E. Perez, L. Schoeffel and L. Favart, (hep-ph/0411389v1)[http://arxiv.org/abs/hep-ph/0411389v1].
[^2]: "A New Monte Carlo Generator for High Energy Physics", S. Kawabata, Comp. Phys. Comm. 41, 127 (1986).
