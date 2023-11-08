---
title: DJANGOH
name: djangoh
category: djangoh
layout: default
---
{% include layouts/title.md %}

  * TOC
  {:toc}

The event generator [DJANGOH](http://www.desy.de/~hspiesb/mcp.html) simulates deep inelastic lepton-proton scattering for both NC and CC events including both QED and QCD radiative effects.

DJANGOH contains the Monte Carlo program HERACLES and an interface of HERACLES to LEPTO. The use of HERACLES allows to take into account the complete one-loop electroweak radiative corrections and radiative scattering.
The LUND string fragmentation as implemented in the event simulation program [JETSET](http://home.thep.lu.se/~torbjorn/Pythia.html) is used to obtain the complete hadronic final state. At low hadronic mass, [SOPHIA](http://ebl.stanford.edu/) is used instead of LEPTO.
DJANGOH comprises the programs (formerly kept separately) DJANGO6 and HERACLES. The interface is to version 6.5.1 of [LEPTO](http://www.isv.uu.se/thep/lepto/).

For the EIC, DJANGOH was upgraded to use nuclear PDFs as available in [LHAPDF](https://lhapdf.hepforge.org/).
From version 4.6.10 on, DJANGOH simulates also longitudinal polarised deep inelastic lepton-proton scattering for both NC and CC events including both QED and QCD radiative effects.


##### Manuals
Detailed documentation on DJANGOH is available from the following sources:
* {% include documents/doc.md name='Djangoh_m.pdf' tag='Manual for DJANGOH 4.6.8' -%}
* {% include documents/doc.md name='Djangoh_Updateh-4.6.10.pdf' tag='How are nuclear PDFs and polarised effects included in Djangoh' -%}
* {% include documents/doc.md name='Lepto-6.5.1.pdf' tag='LEPTO Manual 6.5.1' -%}


##### Output file structure

The output file is in a text format which has the following structure:
* 1st line: <tt>DJANGOH EVENT FILE</tt>
* 2nd line: <tt>============================================</tt>
* 3rd line: Information on event wise variables stored in the file:

| I:                                    | 0 \(line index\)                                                                                                                                                                                              |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ievent:                               | eventnumber running from 1 to XXX                                                                                                                                                                             |
| IChannel:                             | contains information on the origin of the event from the channels of HERACLES \(radiative corrections\)                                                                                                       |
| process \(LST 23\):                   | specifies process simulated \(=2: weak charged current; =4: neutral current\)                                                                                                                                 |
| subprocess \(LST 24\):                | information about first\-order QCD process in current event \(=1: q\-event, =2: qg\-event, =3: q¯q\-event\)                                                                                                   |
| nucleon \(LST 22\):                   | information about chosen target nucleon in current event                                                                                                                                                      |
| struckparton \(LST 25\):              | information about flavor of the struck quark in current event                                                                                                                                                 |
| partontrack \(LST 26\):               | entry line in event record of outgoing struck quark\. In parton shower case, quark at boson vertex before final state shower;                                                                                 |
| y, Q2, x, W2, Nu:                     | are the kinematic variables of the event as calculated from the scattered lepton for NC events\. For CC events it is calculated from the electrons and neutrino\. Includes effects from radiative corrections |
| truey, trueQ2, truex, trueW2, trueNu: | are the kinematic variables of the event at the hard scattering vertex\. Uses the hadronic information \(CC: W\+neutrino\)\. Radiative effects are fully excluded\.                                           |
| SIGtot, errSIGtot                     | total cross section and its uncertainty                                                                                                                                                                       |
| D                                     | depolarisation factor                                                                                                                                                                                         |
| F1NC, F3NC                            | the unpolarised structure functions F1NC~\(q\+antiq\) and F3NC~\(q\-antiq\) for neutral current events                                                                                                        |
| G1NC, G3NC, A1NC                      | the polarised structure functions G1NC~\(dq\+dantiq\) and G3NC~\(dq\-dantiq\) and the simulated asymmetry A1NC=F1NC/G1NC for neutral current events                                                           |
| F1CC, F3CC                            | the unpolarised structure functions for charge current events with W\-: F1CC=\(uptype\+antidowntype\) and F3CC=2\*\(uptype\-antidowntype\); with up <\-> down for W\+                                         |
| G1CC, G5CC                            | the polarised structure functions for charge current events with W\-: G1CC=\(duptype\+dantidowntype\) and G5CC=\(\-duptype\+dantidowntype\); with up <\-> down for W\+                                        |
| nrTracks:                             | number of tracks in this event, includes also virtual particles                                                                                                                                               |
{:.table-bordered}
{:.table-striped}

* 4th line: <tt>============================================</tt>
* 5th line: Information on track-wise variables stored in the file:

| I:        | line index, runs from 1 to nrTracks                                                                       |
|-----------|-----------------------------------------------------------------------------------------------------------|
| K\(I,1\): | status code KS \(1: stable particles 11: particles which decay 55; radiative photon\)                     |
| K\(I,2\): | particle KF code \(211: pion, 2112:n, \.\.\.\.\)                                                          |
| K\(I,3\): | line number of parent particle                                                                            |
| K\(I,4\): | normally the line number of the first daughter; it is 0 for an undecayed particle or unfragmented parton  |
| K\(I,5\): | normally the line number of the last daughter; it is 0 for an undecayed particle or unfragmented parton\. |
| P\(I,1\): | px of particle                                                                                            |
| P\(I,2\): | py of particle                                                                                            |
| P\(I,3\): | pz of particle                                                                                            |
| P\(I,4\): | Energy of particle                                                                                        |
| P\(I,5\): | mass of particle                                                                                          |
| V\(I,1\): | x vertex information                                                                                      |
| V\(I,2\): | y vertex information                                                                                      |
| V\(I,3\): | z vertex information                                                                                      |
{:.table-bordered}
{:.table-striped}
<br />

* 6th line: <tt>============================================</tt>
* 7th line: event information for first event
* 8th line: <tt>============================================</tt>
* 9th to X-1 line: track-wise info of 1st event
* Xth line <tt>============================================</tt>

**For each subsequent event, lines 7 through X repeat analogously.**

##### Unpolarised Example Input

Here is an example input file with settings for unpolarised inelastic neutral-current interactions with radiative corrections turned on.The manual explains the meaning of each input parameter and the associated values.

The differences between DJANGOH 4.6.9 and 4.6.10 are described in detail * {% include documents/doc.md name='Djangoh_Updateh-4.6.10.pdf' tag='here' %}


```
OUTFILENAM
djangoh_20x250_ep.Rad=1.NLO.NC
TITLE
DJANGOH 4.6.10 for eRHIC for PR, NLO at 20x250, Wmin=1.4
EL-BEAM
            20D0      0.0D0    -1
PR-BEAM
            250D0    0.0D0
GSW-PARAM
            2 1 3 1 1 1 2 1 1 1 1
KINEM-CUTS
            3   0D0  1.00D0  0.01D0  0.95D0  1.0D2  1.0D5  1.4D0
EGAM-MIN
            0D0
INT-OPT-NC
            1 18 18 18 18   0 0 0 0
INT-OPT-CC
            0 0 0 0
INT-ONLY
            1
INT-POINTS
            30000
SAM-OPT-NC
            1 1 1 1 1   0 0 0 0
SAM-OPT-CC
            0 0 0 0
NUCLEUS
            250D0   1D0   1D0
NUCL-MOD
            0
STRUCTFUNC
            0  2  10150
POLPDF
            0
LHAPATH
/afs/rhic.bnl.gov/eic/bin/LHAPDF-5.8.6/share/lhapdf/PDFsets
FLONG
            111  0.01  0.03
ALFAS
            1   0   0.20   0.235
FLAVORS
            0   5
IOUNITS
            6  10  11
RNDM-SEEDS
            -1   -1
START
            100000
SOPHIA
            1.5
OUT-LEP
            1
FRAG
            1
CASCADES
            12
MAX-VIRT
            5
CONTINUE
```

To turn radiative corrections off, the following change needs to be made:
```
SAM-OPT-NC
            1 0 0 0 0   0 0 0 0
INT-OPT-NC
           1 0 0 0 0   0 0 0 0
GSW-PARAM
           2 0 3 1 0 0 2 1 1 1 1
```

To run for nuclear targets the following changes need to be made. Any nucleus available in a specific nuclear pdf can be simulated, i.e. D, He, ..., Au,
```
 NUCLEUS
             100.0D0  197 79
 NUCL-MOD
             4201
```

For elastic (e + p &rarr; e + p) and quasi-elastic (e + p &rarr; e + p + &gamma;) neutral current interactions the ```-OPT-NC``` settings should be set as follows:
```
 INT-OPT-NC
            0 0 0 0 0   1 20 20 20
 SAM-OPT-NC
            0 0 0 0 0   1 1 1 1
```

For charged current running, the ```-OPT-NC``` and ```-OPT-CC``` settings should be set as follows:
```
 INT-OPT-NC
             0 0 0 0 0   0 0 0 0
 INT-OPT-CC
             1 20 0 20
 SAM-OPT-NC
             0 0 0 0 0   0 0 0 0
 SAM-OPT-CC
             1 1 0 1
```

##### Polarised Example Input

Here is an example input file with settings for polarized inelastic neutral-current interactions with radiative corrections turned on.
ATTENTION: <br>
to generate W+ exchange: ```EL-BEAM``` needs to be Energy +1.0D0    +1<br>
to generate W- exchange: ```EL-BEAM``` needs to be Energy -1.0D0    -1<br>
the hadron beam can have both helicity states + and -

```
 OUTFILENAM
 djangoh_20x250_proton
 TITLE
 DJANGOH 4.6.10 for eRHIC for PR, LO at 20x250, Wmin=1.4
 EL-BEAM
             20D0    -1.0D0    -1
 PR-BEAM
             250D0   -1.0D0
 GSW-PARAM
             2 1 3 1 1 1 2 1 1 1 1
 KINEM-CUTS
             3   0D0  1.00D0  0.01D0  0.95D0  1.0D2  1.0D5  1.4D0
 EGAM-MIN
             0D0
 INT-OPT-NC
             1 18 18 18 18   0 0 0 0
 INT-OPT-CC
             0 0 0 0
 INT-ONLY
             1
 INT-POINTS
             30000
 SAM-OPT-NC
             1 1 1 1 1   0 0 0 0
 SAM-OPT-CC
             0 0 0 0
 NUCLEUS
             250D0   1   1
 NUCL-MOD
             0
 STRUCTFUNC
             0  2  21000
 POLPDF
             100
 LHAPATH
 /afs/rhic.bnl.gov/eic/bin/LHAPDF-5.8.6/share/lhapdf/PDFsets
 FLONG
             0  0.01  0.03
 ALFAS
             1   0   0.20   0.235
 NFLAVORS
             0   5
 IOUNITS
             6  10  11
 RNDM-SEEDS
             -1   -1
 START
             100000
 SOPHIA
             1.5
 OUT-LEP
             1
 FRAG
             1
 CASCADES
             1
 MAX-VIRT
             5
 CONTINUE
 ```

There is also the option to simulate a polarized neutron target by using
```
 NUCLEUS
             250D0   1D0   0D0
```
