---
title: BeAGLE
name: beagle
category: beagle
layout: default
---

{% include layouts/title.md %}
* TOC
{:toc}


#### About

BeAGLE - **B**enchmark **eA** **G**enerator for **LE**ptoproduction

**[Latest version 1.01.04](https://gitlab.in2p3.fr/BeAGLE/BeAGLE)**

<left>
{% include images/image.md name='beagleAsciiLogo' width='400' %}
</left>


**Contacts:**
* Mark Baker <mdbaker@bnl.gov>
* Kong Tu <zhoudunming@bnl.gov>


#### Program Overview

BeAGLE uses the [DPMJet](https://wiki.bnl.gov/eic/index.php/DPMJet) framework in order to handle the target
(Glauber, FLUKA etc.) for ep and eA collisions but employs [PYTHIA](https://eic.github.io/software/pythia6.html)
to handle the elementary interaction as a replacement for PHOJET. The nuclear PDF is included by linking the
PYTHIA PDF library to LHAPDF which enables us access to EPS09 nuclear PDF routines. We have included the
energy loss module PyQM, written by Accardi and Dupre and based on the quenching weights of
Salgado & Wiedemann. This module, when turned on, applies energy loss to the partons after they have
been simulated by Pythia, but before they have been hadronized.

The original DPMJet models DTUNUC, PHOJET, QNEUTRIN are NOT available in BeAGLE.
If you are interested in these models for pp/pA, real photoproduction, or quasi-elastic neutrino scattering,
use [DPMJet](https://wiki.bnl.gov/eic/index.php/DPMJet) directly. The history and motivation of the use of
DPMJetHybrid rather than DPMJet for eRHIC can be
found [here](https://wiki.bnl.gov/eic/index.php/DpmjetHybrid#Opening_issue).

A graphical demonstration of the program structure can be illustrated as follows:
<center>
{% include images/image.md name='beagle_design' width='732' %}
</center>

#### Basic Model
The program can be run in, essentially, two modes.

Setting ```genShd=1``` (generator Shadowing=1) is similar to DPMJetHybrid. The entire nuclear shadowing
effect (where R=σ(A)/Aσ(p)<1) is interpreted as a change in the parton distribution of unspecified origin,
but the DIS interaction is assumed to be point-like. One and only one nucleon participates in the interaction.

Note: This approach is in stark contrast to the original DPMJET approach which used GMVD (Generalized Vector Meson Dominance) and allowed multiple collisions.

Setting ```genShd>1``` (specifically 2 or 3) activates a very simplistic model of nuclear shadowing loosely based on the dipole-model or aligned-jet model. Even for DIS events, the scattering process in the target rest frame is viewed as being due to a rare hadronic fluctuation of the virtual photon with a very large cross-section (few mb). In this case, the **entire** R(x,Q2)<1 shadowing effect is attributed to multiple nucleon collisions. Viewed in the hadronic center-of-mass frame, such collisions can still look like conventional DIS.

We implement this by making a map, outside of BeAGLE, using ```TGlauberMC``` between the "dipole" cross-section σ and the value of R. When running BeAGLE, we look up R in the nPDF (such as EPS09) and find the corresponding dipole cross-section. Using the dipole cross-section, we can then simulate the collisions with Glauber, leading to a variety of ```Ncoll``` values. If ```Ncoll>1```, we currently pick one of the struck nucleons to undergo a Pythia interaction (for ```genShd=2```, this is a random choice among the struck nucleons; for ```genShd=3``` it is the first struck nucleon seen by the γ<sup>*</sup>). Any additional struck nucleons undergo an elastic scatter with the most forward parton (in the direction of the γ<sup>*</sup>) with a pt magnitude of a typical intrinsic kt.

It should be noted that for x<sub>Bj</sub>>0.1 or R>1, we revert to ```genShd=1``` behavior.

At the moment, there are many simplifying assumptions for the multinucleon mode, made for convenience, which we hope to systematically relax as we upgrade the program. These include:



> Infinite coherence length even at modest x. <br/>
> We apply an overall multinucleon effect based on the R<sub>sea</sub>(x,Q<sup>2</sup>) independent of process. <br/>
>    Note: Processes such as Photon-Gluon Fusion should depend of R<sub>G</sub> while diffraction should INCREASE as R gets smaller. <br/>
> We assume that the full shadowing effect is due to multinucleon collisions with no modification of individual nucleons.

#### How to run BeAGLE, step-by-step instructions
*<font color="blue">Contact persons: Mark Baker (mdbaker@bnl.gov), Kong Tu(zhoudunming@bnl.gov)</font>*


**<font size="5">Overview</font>**

There are two central managed directory/repo stored at BNL,
* PACKAGES:
````
/afs/rhic/eic/PACKAGES/BeAGLE
````
* Gitstore:
````
/afs/rhic/eic/gitstore/BeAGLE
````

Never run BeAGLE from these two directories and please follow the instructions below. These two directories are kept exactly identical. The ``PACKAGES`` is the official version, while the gitstore version could be updated by the developers from time to time.

**<font size="5">Users only</font>**

Make a new directory, e.g., run-BeAGLE-Name,
````
mkdir run-BeAGLE-Kong 
cd run-BeAGLE-Kong
````

Copy the fluka nuclear data file:
````
cp /afs/rhic/eic/PACKAGES/BeAGLE/nuclear.bin . (for BNL)
cp /u/group/ldgeom/PACKAGES/BeAGLE/nuclear.bin . (for JLAB)
````

Source setup_BeAGLE to setup your environment variable $BEAGLESYS, which will point to the BeAGLE directory, e.g., the PACKAGES directory. One can also source the same thing under the gitstore version.
````
source /afs/rhic/eic/PACKAGES/BeAGLE/setup_BeAGLE (for BNL) 
source /u/group/ldgeom/PACKAGES/BeAGLE/setup_BeAGLE (for JLAB)
````
when done, ``$BEAGLESYS/BeAGLE`` will be the executable.

**<font size="5">Developers</font>**

Make a new directory, e.g., BeAGLE-dev-Name-Date,
````
mkdir BeAGLE-dev-Kong-2020-01-29 
cd BeAGLE-dev-Kong-2020-01-29
````

git clone the developer's area,
````
git clone /afs/rhic/eic/gitstore/BeAGLE
cd BeAGLE
cp /afs/rhic/eic/gitstore/BeAGLE/RAPGAP-3.302/lib/*.a ./RAPGAP-3.302/lib/
make all
````

The executable will be under the main directory, called "BeAGLE". So instead of
running the executable from central directory, one runs from here locally. See later on ``running instructions.``

**<font size="5">Running the code</font>**

Copy your own input files, there are at least two of them are necessary,
BeAGLE input control card, \*.inp, and Pythia/Rapgap input control card, S\*, e.g, S3ALL003
````
cp -rf /gpfs02/eic/ztu/BeAGLE/BeAGLE_examples-2022-01-11/inputFiles ./ 
cp /gpfs02/eic/ztu/BeAGLE/BeAGLE_examples-2022-01-11/S* ./
````

Create your own output directory,
````
mkdir outForPythiaMode
````

Copy and paste the macro to convert text output to root output, e.g.,
````
cp /gpfs02/eic/ztu/BeAGLE/BeAGLE_examples-2022-01-11/BuildIt.C ./
````

To run BeAGLE, do this following command under the main directory,
* for users running:
````
$BEAGLESYS/BeAGLE < inputFiles/yourinput.inp > log_file_name.txt
````
* for developers running locally:
````
./BeAGLE < inputFiles/yourinput.inp > log_file_name.txt
````
the output will be under outForPythiaMode, with a common name. Overwrite the name to xxx.txt.

Go back to main directory, and run:
````
root -b -q 'BuildIt.C("xxx.txt")'
````
the root file will have the same name as the txt file.


To analyze the root file, one can loop over events AND particles
and do analysis. One example macro is provided under
````
/gpfs02/eic/ztu/BeAGLE/BeAGLE_examples-2022-01-11/runEICTree.C
````

Grab necessary parts for what's needed.
Then simply do:
````
root -l runEICTree.C+\(\"filename\"\) 
````

The filename is without the .root extension.
Note: In order to access EventBeagle (which is defined under EventPythia.h), and besides to setup eic_cshrc, one needs to setup include path to /afs/rhic.bnl.gov/eic/include, For example, if one is not sure, open root, and try,
````
gSystem->GetIncludePath()
````
See if ``-I/afs/rhic.bnl.gov/eic/include`` was added. If not, you need to add the include path.


#### BeAGLE Control Card
Note: This input file (originally from DPMJET) is very particular about the spacing of a lot of the numbers so make sure that you leave the starting space the same when you change numbers. In particular, the input format is: A10, 6E10.0, A8

Frequently changed cards:
```
START    - the first # is the # of events requested.
TARPAR   - the first # is A and the second # is Z for the target
           the third # is the n/p handling mode for the Pythia subevent:
                0=sequential n,p. The first ~N/A events are en, the remaining ep (binomial prob.)
                                  Useful for getting en & ep cross-sections from Pythia
                1=en only test mode (not really for physics)
                2=ep only test mode (not really for physics)
                3=random mix. The events are randomly en or ep.
                              Useful because you can analyze just a subset of the data without bias.
TAUFOR   - the first # is the formation time parameter (tau) in fm/c for the intranuclear cascade
           the second # is the number of generations followed Default=25, 0 means no cascade
MOMENTUM - the first # is for the lepton beam (GeV/c), the second for the A (or p)
           both #s should be entered as positive, but the lepton beam will be multiplied by -1.
L-TAG    - these #s are cuts: yMin yMax Q2Min Q2Max thetaMin thetaMax
           where y & Q2 (GeV2) are the leptoproduction kinematics
           and theta (radians) refers to the lepton scattering angle in the laboratory frame.
PY-INPUT - specifies the file (with an eight-character maximum name!) used as a pythia input file.   
           Examples (such as eAS2noq) can be found in the /afs/rhic/EIC/PACKAGES/BeAGLE/ directory.
FERMI    - First # means: -1 = no Fermi motion at all
                           1 = DPMJET Fermi motion, but Pythia subevent neglects it (DPMJetHybrid mode)
                           2 = Fermi motion added to Pythia subevent after the fact
                           3 = Pythia subevent used correct Fermi motion (Not Yet Implemented)
           Second #: "Scale factor" for Fermi momentum distribution in GeV (0.62 is default, don't touch it unless instructed to do so)  
           Third #: Fermi momentum distribution 0 (D) = Most recommended distribution
           Fourth #: Post-processing flag:  (Not Yet Implemented)
                           0 (D) = no post-processing
                           1     = Fix energy non-conservation in ion frame (for small nuclei)
FSEED     - Uncomment/comment this line for debugging/production
OUTLEVEL  - First 4 #s are verbosity flags -1=quiet, >=1 increasing verbosity
            Fifth # is the number of events to print out and in some cases to be verbose about
PYVECTORS - Allowed Pythia vector mesons for diffraction: 0(D)=all, 1=rho, 2=omega, 3=phi, 4=J/psi
USERSET   = First # specifies the meaning of the variables User1,User2,User3 (detailed below)
            Second # specifies the maximum excitation energy in GeV handed to Fluka (D=9.0)
                      Note: This should not come into play, but it protects against infinite loops.
PHOINPUT  - Any options explained in the PHOJET-manual can be used in between
            the "PHOINPUT" and "ENDINPUT" cards.
PROCESS   -       1 1 1 1 1 1 1 1
ENDINPUT  -

The last two cards should be:
START     = First # specifies the # of events to run
            Second # should be 0 (or missing)
STOP
```
The ``USERSET`` are defined as in this [link.](https://gitlab.in2p3.fr/BeAGLE/BeAGLE/-/blob/master/src/dpmjet3.0-5F-new.f?expanded=true&viewer=simple#L2386) Be patient, it will need 30 secs to load the source code. For example, ``USERSET==16`` is to save the information of the Deuteron wavefunction. 


<font size="5">PYTHIA Control Card</font>

The format of the PY-INPUT file is similar to the Pythia input file, which is not really documented elsewhere, but is fairly self-explanatory due to the comments. See below.

```yaml
line 1:    output file name for the textfile
line 2:    xmin and xmax
line 3:    ignored unless radcor is being used
line 4: 0 switch for radcor. 0=off. 1=on. 2=generate tables. radcor>0 NOT YET TESTED.
line 5: 1 GVMD model choice 0=x,Q2, 1=y,Q2
line 6: 1,1  A-Tar and Z-Tar really only used if radcor is used.
line 7:   Shadowing/multinucleon switch genShd for BeAGLE. 1=1 nucleon struck.
          2=Glauber. Pythia-interacting nucleon chosen at random from struck nucleons. All others elastic scatter.
          3=like 2, but Pythia-interaction is the first (most negative z in TRF with +z along γ* direction) nucleon.
line 8: 101  Means LO with error set 1 in the nuclear PDF.
line 9:      switch for quenching (1:on, 0:off) NOTE: quenching is NOT WELL TESTED yet.
line 10:     Qhat value for quenching (0.0 also means no quenching even if the switch=1)
OPTIONAL LINE: FSEED ####### - Fixes seed for random # generator for debugging. Don't use for production.
```
Other lines make changes to PYTHIA control parameters.

The usage and explanations for the parameters in the input file and what kind of parameters do we need to give for PYTHIA can be found in the following file:
```bash
/afs/rhic/eic/PACKAGES/BeAGLE/Documents/Documentation
```
To find the final state particles, just use the following selection rules:
```
ISTHKK(i)=1
```
which selects all the particles from hard interaction, nuclear evaporation and heavy nuclear fragments.

#### Output Data Format:
The BeAGLE output is a text file intended to be read in by eic-smear and EicRoot at BNL and by GEMC at JLAB.

For the output file, we propose the following text format & structure:
* 1st line: <tt>PYTHIA</tt> EVENT FILE
* 2nd line: <tt>============================================</tt>
* 3rd line: Information on event wise variables stored in the file:

| I:                                                                                                                                        | I | 0 \(line index\)                                                                                                                                                                  |
|-------------------------------------------------------------------------------------------------------------------------------------------|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ievent:                                                                                                                                   | I | eventnumber running from 1 to XXX                                                                                                                                                 |
| genevent:                                                                                                                                 | I | trials to generate this event                                                                                                                                                     |
| ltype:                                                                                                                                    | I | particle code for incoming lepton                                                                                                                                                 |
| Atarg:                                                                                                                                    | I | A of the target nucleus                                                                                                                                                           |
| Ztarg:                                                                                                                                    | I | Z of the target nucleus                                                                                                                                                           |
| pzlep:                                                                                                                                    | D | lab pz of the lepton beam \(sign\(pz\)\*p for non\-zero crossing angle\)                                                                                                          |
| pztarg:                                                                                                                                   | D | lab pz/A of the ion beam \(sign\(pz\)\*p/A for non\-zero crossing angle\)                                                                                                         |
| pznucl:                                                                                                                                   | D | lab pz of the struck nucleon \(sign\(pz\)\*p for non\-zero crossing angle\)                                                                                                       |
| crang:                                                                                                                                    | D | crossing\-angle \(mr\)\. ** NOT IMPLEMENTED YET ** crang = \(1000\*\) atan\(px/pz\) for the beam momentum with nonzero px\. Note: we assume one of the beams has px=py=0 and the other py=0\.               |
| crori:                                                                                                                                    | I | crossing angle orientation. ** NOT IMPLEMENTED YET ** +/-1 lepton beam defines +/- z direction. +/-2 ion beam defines +/- z direction. 0 no crossing angle.|
| subprocess:                                                                                                                               | I | pythia subprocess \(MSTI\(1\)\), for details see PYTHIA                                                                                                                           |
| nucleon:                                                                                                                                  | I | hadron beam type \(MSTI\(12\)\)                                                                                                                                                   |
| targetparton:                                                                                                                             | I | parton hit in the target \(MSTI\(16\)\)                                                                                                                                           |
| xtargparton:                                                                                                                              | D | x of target parton \(PARI\(34\)\)                                                                                                                                                 |
| beamparton:                                                                                                                               | I | in case of resolved photon processes and soft VMD the virtual photon has a hadronic structure\. This gives the info which parton interacted with the target parton \(MSTI\(15\)\) |
| xbeamparton:                                                                                                                              | D | x of beam parton \(PARI\(33\)\)                                                                                                                                                   |
| thetabeamparton:                                                                                                                          | D | theta of beam parton \(PARI\(53\)\)                                                                                                                                               |
| truey, trueQ2, truex, trueW2, trueNu:                                                                                                     | D | are the kinematic variables of the event\.                                                                                                                                        |
| | | If radiative corrections are turned on they are different from what is calculated from the scattered lepton\.                             |
| | | If radiative corrections are turned off they are the same as what is calculated from the scattered lepton                                 |
| leptonphi:                                                                                                                                | D | phi of the lepton \(VINT\(313\)\) in the laboratory frame                                                                                                                         |
| s\_hat:                                                                                                                                   | D | s\-hat of the process \(PARI\(14\)\)                                                                                                                                              |
| t\_hat:                                                                                                                                   | D | Mandelstam t \(PARI\(15\)\)                                                                                                                                                       |
| u\_hat:                                                                                                                                   | D | Mandelstam u \(PARI\(16\)\)                                                                                                                                                       |
| pt2\_hat:                                                                                                                                 | D | pT2\-hat of the hard scattering \(PARI\(18\)\)                                                                                                                                    |
| Q2\_hat:                                                                                                                                  | D | Q2\-hat of the hard scattering \(PARI\(22\)\),                                                                                                                                    |
| F2, F1, R, sigma\_rad, SigRadCor:                                                                                                         | D | information used and needed in the radiative correction code                                                                                                                      |
| EBrems:                                                                                                                                   | D | energy of the radiative photon in the nuclear rest frame                                                                                                                          |
| photonflux:                                                                                                                               | D | flux factor from PYTHIA \(VINT\(319\)\)                                                                                                                                           |
| b:                                                                                                                                        | D | Impact parameter \(in fm\), radial position of virtual photon w\.r\.t\. the center of the nucleus in the nuclear TRF with z along γ\*                                             |
| Phib:                                                                                                                                     | D | azimuthal angle of the impact parameter of the virtual photon in the nuclear TRF with z along γ\* and φ=0 for the scattered electron                                              |
| Thickness:                                                                                                                                | D | Nuclear thickness T\(b\)/ρ0 in units of fm\.                                                                                                                                      |
| ThickScl:                                                                                                                                 | D | Nuclear thickness T\(b\) in nucleons/fm2\.                                                                                                                                        |
| Ncollt:                                                                                                                                   | I | Number of collisions between the incoming γ\* and nucleons in the nucleus \(same as number of participating nucleons\)                                                            |
| Ncolli:                                                                                                                                   | I | Number of inelastic collisions between the incoming γ\* and nucleons in the nucleus \(same as number of inelastically participating nucleons\)                                    |
| Nwound:                                                                                                                                   | I | Number of wounded nucleons, including those involved in the intranuclear cascade\. WARNING: This variable is not finalized yet                                                    |
| Nwdch:                                                                                                                                    | I | Number of wounded protons, including those involved in the intranuclear cascade\. WARNING: This variable is not finalized yet                                                     |
| Nnevap:                                                                                                                                   | I | Number of evaporation \(and nuclear breakup\) neutrons                                                                                                                            |
| Npevap:                                                                                                                                   | I | Number of evaporation \(and nuclear breakup\) protons                                                                                                                             |
| Aremn:                                                                                                                                    | I | A of the nuclear remnant after evaporation and breakup                                                                                                                            |
| Ninc:                                                                                                                                     | I | Number of stable hadrons from the Intra Nuclear Cascade                                                                                                                           |
| Nincch:                                                                                                                                   | I | Number of charged stable hadrons from the Intra Nuclear Cascade                                                                                                                   |
| d1st:                                                                                                                                     | D | density\-weighted distance from first collision to the edge of the nucleus \(amount of material traversed / ρ0\)                                                                  |
| davg:                                                                                                                                     | D | Average density\-weighted distance from all inelastic collisions to the edge of the nucleus                                                                                       |
| pxf,pyf,pzf:                                                                                                                              | D | Fermi\-momentum of the struck nucleon \(or sum Fermi momentum for all inelastic nucleon participants for Ncolli>1\) in target rest frame with z along gamma\* direction           |
| Eexc:                                                                                                                                     | D | Excitation energy in the nuclear remnant before evaporation and breakup\.                                                                                                         |
| RA                                                                                                                                        | D | Nuclear PDF ratio for the up sea for the given event kinematics \(x,Q2\), but set to 1 if multi\-nucleon shadowing is off \(genShd=1\)                                            |
| User1,User2,User3:                                                                                                                        | D | User variables to prevent/delay future format changes                                                                                                                             |
| nrTracks:                                                                                                                                 | I | number of tracks in this event, including event history                                                                                                                           |
{:.table-bordered}
{:.table-striped}

* 4th line: <tt>============================================</tt>
* 5th line: Information on track-wise variables stored in the file:

| I:             | I | line index, runs from 1 to nrTracks                                                                                                        |
|----------------|---|--------------------------------------------------------------------------------------------------------------------------------------------|
| ISTHKK\(I\):   | I | status code KS: KS=1 is the only stable final state particle code, Use NoBAM variable \(below\) to specify origin of particle              |
| IDHKK\(I\):    | I | particle KF code \(211: pion, 2112:n, \.\.\.\.\)\. Code 80000 refers to a nucleus, specified in more detail by A=IDRES\(I\), Z=IDXRES\(I\) |
| JMOHKK\(2,I\): | I | line number of second mother particle                                                                                                      |
| JMOHKK\(1,I\): | I | line number of first mother particle                                                                                                       |
| JDAHKK\(1,I\): | I | normally the line number of the first daughter\.                                                                                           |
| JDAHKK\(2,I\): | I | normally the line number of the last daughter\.                                                                                            |
| PHKK\(1,I\):   | D | px of particle \(GeV/c\)                                                                                                                   |
| PHKK\(2,I\):   | D | py of particle \(GeV/c\)                                                                                                                   |
| PHKK\(3,I\):   | D | pz of particle \(GeV/c\)                                                                                                                   |
| PHKK\(4,I\):   | D | Energy of particle \(GeV\)                                                                                                                 |
| PHKK\(5,I\):   | D | mass of particle \(GeV/c^2\)                                                                                                               |
| VHKK\(1,I\):   | D | x vertex information \(mm\)                                                                                                                |
| VHKK\(2,I\):   | D | y vertex information \(mm\)                                                                                                                |
| VHKK\(3,I\):   | D | z vertex information \(mm\)                                                                                                                |
| IDRES\(I\):    | I | Baryon number, or A for a nucleus \(IDHKK\(I\)=80000\), fractional B set to 0\.                                                            |
| IDXRES\(I\):   | I | Particle charge, \(Z for a nucleus\), 0 for fractional charge\.                                                                            |
| NOBAM\(I\):    | I | Flag describing the particle origin, particularly for final state particles\.                                                              |
{:.table-bordered}
{:.table-striped}

* 6th line: <tt>============================================</tt>
* 7th line: event information for first event. Format descriptor:

```
format ((I4,1x,$),(I10,1x,$),4(I4,1x,$),4(f12.6,1x,$),3(I4,1x,$),
&     I6,1x,$,f9.6,1x,$,I6,1x,$,2(f12.6,1x,$),7(f18.11,3x,$),
&     11(f19.9,3x,$),4(f10.6,1x,$),9(I5,1x,$),2(f10.6,1x,$),
&     3(f15.6,1x,$),2(f9.6,1x,$),3(e17.8,1x,$),I6,/)
```
* 8th line: <tt>============================================</tt>
* 9th to X-1 line: track-wise info of 1st event. Format descriptor:

```
format (2(I6,1X,$),I10,1x,$,4(I8,1x,$),5(f15.6,1x,$),3(e15.6,1x,$),3(I8,1x,$)/)
```

* Xth line <tt>============================================</tt>

**The information from line 7 to X repeats for each event.**

Something to be mentioned here is the first 4 tracks in every event which deliver the information of beam parameter are added externally in order to get some handy variables like z, ptVsGamma... So the mother daughter relationship of these four tracks can not be taken too seriously. But the orig number and daughter number of the tracks after those 4 tracks are consistent and can be used for further purposes.

The 4-momentum for particle I, PHKK(mu,I) is reported in the lab frame for all stable final state particles. It is also reported in the lab frame for most intermediate/document particles with the following exceptions: Particle 5 is a copy of the γ<sup>*</sup> with 4-momentum reported as zero. Particles 6 through A+5 are the incoming original nucleons from the A, in the target rest frame with γ<sup>*</sup> momentum along +z. The momentum corresponds to the Fermi momentum of the nucleon in question.

The impact parameter, defined as the transverse position of the virtual photon with respect to the nucleus center in a frame with z along γ<sup>*</sup>, is given by bx=VHKK(1,5) and by=VHKK(2,5) in mm.

The status codes ISTHKK(I) have the following meaning. Note that final state particles have status 1 (although internally -1 and 1001 are also used). All other statuses refer to intermediate particles or informational lines.

| \(\-1\):  | Internally used for final state particle from excited nucleus evaporation and breakup\. Output as KS=1,NOBAM=3 |
| 1:        | stable final state particle, Note: internally, \-1 and 1001 are also used\.                                    |
| 2:        | unstable intermediate particle                                                                                 |
| 3:        | Documentation line describing the collision                                                                    |
| 11:       | Projectile \(γ\*\) documentation line                                                                          |
| 12:       | Incoming wounded nucleon from nucleus                                                                          |
| 14:       | Incoming spectator nucleon from nucleus                                                                        |
| 16:       | Intermediate baryon involved in the intranuclear cascade                                                       |
| 18:       | Spectator nucleon \(wrt hard collision\) involved in the intranuclear cascade                                  |
| 19:       | Intermediated meson involved in the intranuclear cascade                                                       |
| 21:       | documentation line describing the collision                                                                    |
| 1000:     | excited nucleus, which will evaporate and breakup                                                              |
| \(1001\): | Internally used to mean residual nucleus\. Output as KS=1, NOBAM=4                                             |
{:.table-bordered}
{:.table-striped}

<br />
The NOBAM particle origin flag for final state particles has the following meaning:

| 0,2:    | Created in the hard collision process\.                                                                        |
| 3:      | Created during the evaporation/breakup phase \(formerly specified by KS=\-1\)                                  |
| 4:      | Largest remnant nucleus created during the evaporation/breakup phase \(formerly specified by KS=1001\)\.       |
| 11\-34: | Particles created during the Intra\-nuclear cascade\. Cascade generation \(IDCH\) \#n specified by NOBAM=10\+n |
{:.table-bordered}
{:.table-striped}

<br />
<br />
<br />
<br />
<br />
<br />
