---
title: PEPSI
name: pepsi
category: pepsi
layout: default
---
{% include layouts/title.md %}

**Note: By now it is better to use DJANGOH for polarized studies as it is the more complete generator**

  * TOC
  {:toc}

PEPSI (Polarized Electron Proton Scattering Interactions) is a Monte Carlo generator for polarized deep inelastic scattering (pDIS). It is based on the LEPTO 6.5 Monte Carlo for unpolarized DIS.

See L. Mankiewicz, A. Schäfer and M. Veltri, Comp. Phys. Comm. 71, 305-318 (1992),
{% include navigation/findlink.md name='pepsi_paper' tag='PEPSI.paper.pdf' %}

#### Parton distribution functions

The distribution function to use in polarized leptoproduction is set via the variable <tt>LST(15)</tt> in the LEPTO <tt>COMMON</tt> block <tt>/LEPTOU/</tt>.
[Table 1](#table-1-polarized-parton-distributions) and [Table 2](#table-2-unpolarized-parton-distributions) list internal allowed values of <tt>LST(15)</tt> for polarized and unpolarized distributions respectively.

Pepsi is linked with the pdflib such that all PDFs included in there can be used by setting <tt>LST(15)</tt> to the respective PDF-ID

##### PEPSI processes important in ep

| Subprocess | \# | Description |
|:---:|:---:|:---:|
| DIRECT |||
| γ<sup>*</sup>q → q | 1 | LO DIS |
| γ<sup>*</sup>q → qg | 2| QCDC |
| γ<sup>*</sup>g → q qbar | 3| PGF |
{:.table-bordered}
{:.table-striped}

<br />
*QCDC*: QCD-Compton, radiation of a gluon from incoming or outgoing quark lines
<br />
*PGF*: Photon Gluon Fusion

#### Running PEPSI
In the standard setup in the singularity cvmfs environment or at BNL, the code can be found in
```bash
$EICDIRECTORY/PACKAGES/pepsi
```

The executables are in the same directory and called `pepsieRHICnoRAD` and `pepsieRHICwithRAD`, respectively. There are several steer files (named `input.data.XXXXX.eic`) provided in this directory to run PEPSI and get reasonable output.

PEPSI has to be run twice if polarized asymmetries should be generated, once for parallel, lepton and proton beam spin direction, and once antiparallel.
Charge Current events can only be generated in the unpolarized mode.
The <tt>LST(8)</tt> can only be different from 0 or 1 in the unpolarized mode.

Note that the executables expect the `pdf/` directory in the directory of execution. Easiest way to achieve this is a softlink (adapt to your location)
```bash
ln -s $EICDIRECTORY/PACKAGES/PEPSI/pdf
```

##### Without radiative corrections
Choose or create a steering file. Some that are provided in `$EICDIRECTORY/PACKAGES/pepsi/STEER-FILES`:
* `input.data_noradcor.eic.pol.anti` is an example to run PEPSI with settings tuned for Hermes, and/or H1 and ZEUS for the antiparallel polarized cross-section
* `input.data_noradcor.eic.pol.par`: for the parallel polarized cross-section
* `input.data_noradcor.eic.unpol`: for the unpolarized cross-section

You can then run:
```bash
./pepsieRHICnoRAD < STEER-FILES/input.EW_noradcor.eic.posi.test > XXX.log
```
where the output redirect to `XXX.log` is optional.

##### With radiative corrections

**Note: DO NOT USE radiative corrections. Currently this is no longer supported. See [Known Issues](#known-issues)**

##### Output file structure

The output file is in a text format which has the following structure:
* 1st line: <tt> PEPSI EVENT FILE</tt>
* 2nd line: <tt>============================================</tt>
* 3rd line: Information on event wise variables stored in the file:

| I:                                                                                 | 0 (line index)                                                                                               |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| ievent:                                                                            | eventnumber running from 1 to XXX                                                                            |
| genevent:                                                                          | trials to generate this event                                                                                |
| process:                                                                           | pepsi subprocess (LST(23)), for details see table above                                                      |
| subprocess:                                                                        | pythia subprocess (LST(24)), for details see table above                                                     |
| nucleon:                                                                           | hadron beam type (LST(22))                                                                                   |
| struckparton:                                                                      | parton hit in the target (LST(25))                                                                           |
| partontrack:                                                                       | \# or parton track (LST(26))                                                                                 |
| truey, trueQ2, truex, trueW2, trueNu:                                              | are the kinematic variables of the event.                                                                    |
|                                                                                    | If radiative corrections are turned **on** they are **different** from what is calculated from the scattered lepton. |
|                                                                                    | If radiative corrections are turned **off** they are **the same** as what is calculated from the scattered lepton    |
| mcfixedweight                                                                      | weight calculated from generation limits                                                                     |
| weight                                                                             | total weight including everything                                                                            |
| dxsec                                                                              | cross section included in the weight                                                                         |
| mcextraweight                                                                      | Pepsi total cross section in pb from numerical integration parl(23)                                          |
| dilut, F1, F2, A1, A2, R, Depol, d, eta, eps, chi                                  | true variables needed to calculate g1                                                                        |
| gendilut, genF1, genF2, genA1, genA2, genR, genDepol, gend, geneta, geneps, genchi | variables needed to calculate g1                                                                             |
| SigRadCor:                                                                         | information used and needed in the radiative correction code                                                 |
| EBrems:                                                                            | energy of the radiative photon in the nuclear rest frame                                                     |
| nrTracks:                                                                          | number of tracks in this event, includes also virtual particles<br><br>                                      |
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

**For each subsequent event, lines 7 through X repeat analogously   .**

##### How to analyze events

The recommended way is to create and use a ROOT tree with the `BuildTree` function and other tools provided by [eic-smear](eicsmear.html).
Some guidelines regarding Monte Carlo normalization can be found [here](https://wiki.bnl.gov/eic/index.php/Simulations#MC_Analysis_Techniques).

<br />
<br />
<br />

#### Installation

It is recommended to take advantage of the pre-installed versions on the lab farms or
the available stand-alone [singularity](eicsmear_generators_singularity.html) or [escalate](escalate_singularity_1.html) containers.

However, the package can also be built using "make".
The Makefile should be customized to your environment before using,
specifically you need to adapt the "install" target and point to a compatible CERNLIB installation.

Ignore warnings of the form:
```
 Warning: $ should be the last specifier in format
```
 Which should be okay (it is a g77 extension allowed by gfortran).
 As well as:
```
Warning: Deleted feature: PAUSE statement at (1)
```
This feature is deleted in F95; here, it should eventually be replaced by write() + read().

##### Changes made for more recent fortran versions
Multiple warnings like this:
```pepsi/setctq5.F:9.10:
     >   './pdf/cteq5hj.tbl',                                           
          1
Warning: Initialization string starting at (1) was truncated to fit the variable (16/17)
```

indicate a too short variable length. Changed to
```fortran
      Character Flnm(Isetmax)*100
```
in line 6 (of setctq5.F).

#### Known Issues
**Note: DO NOT USE radiative corrections. Currently does not work.**

If radiative corrections are restored, this would be the procedure to use them:

* Create a directory called `radgen` in the area you want to run the code.
* You either need to generate the lookup table for your cuts and beam energy settings first
```
   pepsieRHICwithRAD < input.data_make-radcor.eic.unpol
```
or you can use one of the files already generated in the directory
`$EICDIRECTORY/PACKAGES/pepsi/radgen`
and called `xytab1unp.04.050.dat` or `xytab1ant.04.050.dat` or `xytab1par.04.050.dat`.
* To run the code than with radiative corrections simply change the steer file to either `input.data_radcor.eic.unpol` or `input.data_radcor.eic.pol.par` or ... and run
```
   pepsieRHICwithRAD < input.data_radcor.eic.unpol > XXX.log
```

However, as noted above, they cannot currently be used.
```
./pepsieRHICwithRAD < STEER-FILES/input.data_radcor.eic.pol.anti

At line 514 of file pepsiMaineRHIC_radcorr.v2.F (unit = 29, file =
'pepsi.ep.4x100.1Mevents.pol-anti.RadCor=1.txt')
Fortran runtime error: Expected REAL for item 42 in formatted
transfer, got INTEGER
```

Details about radiative corrections can be found [here](pythia6.html/#radiative-corrections).

<br />

#### Table 1: Polarized parton distributions

| LST(15) | Polarized parton distribution function                                                                                   |
| ------- | ------------------------------------------------------------------------------------------------------------------------ |
| 101     | Schaefer, Phys. Lett. B 208,2 (1988) 175 for comparison with older PEPSI versions                                        |
| 102     | free                                                                                                                     |
| 103     | free                                                                                                                     |
| 104     | Schaefer et al hep-ph/9505306 using Glueck et al. Z. Phys. C 53 (1992) 127                                               |
| 105     | Bartelski et al hep-ph/9502271 Set II(p,n)                                                                               |
| 106     | Bartelski et al hep-ph/9502271 Set II(P,n)                                                                               |
| 107     | Gehrmann hep-ph/9512406 Gluon A (NLO) + DGLAP                                                                            |
| 108     | Gehrmann hep-ph/9512406 Gluon A (NLO) + DGLAP                                                                            |
| 109     | Gehrmann hep-ph/9512406 Gluon A (NLO) + DGLAP                                                                            |
| 110     | Gehrmann et al hep-ph/9512406 Gluon A (LO)                                                                               |
| 111     | Gehrmann et al hep-ph/9512406 Gluon B (LO)                                                                               |
| 112     | Gehrmann et al hep-ph/9512406 Gluon C (LO)                                                                               |
| 113     | Gehrmann et al hep-ph/9512406 Gluon A (LO) + (DGLAP)                                                                     |
| 114     | Gehrmann et al hep-ph/9512406 Gluon B (LO) + (DGLAP)                                                                     |
| 115     | Gehrmann et al hep-ph/9512406 Gluon C (LO) + (DGLAP)                                                                     |
| 116     | M. Glueck, E. Reya, M. Stratmann and W. Vogelsang, DO-TH 95/13, RAL-TR-95-042 'standard' scenario, next-to-leading order |
| 117     | M. Glueck, E. Reya, M. Stratmann and W. Vogelsang, DO-TH 95/13, RAL-TR-95-042 'valence' scenario, next-to-leading order  |
| 118     | M. Glueck, E. Reya, M. Stratmann and W. Vogelsang, DO-TH 95/13, RAL-TR-95-042 'standard' scenario, leading order         |
| 119     | M. Glueck, E. Reya, M. Stratmann and W. Vogelsang, DO-TH 95/13, RAL-TR-95-042 'valence' scenario, leading order          |
| 120     | Stanley J.Brodsky Nucl.Phys. B441(1995)                                                                                  |
| 121     | S.Keler & J.F.Owens Phys.Lett. B266(1991) & Phys.Rev. D19(1994)1199                                                      |
| 124     | D. de Florian et al., hep-ph/9711440 LO set 1                                                                            |
| 125     | D. de Florian et al., hep-ph/9711440 LO set 2                                                                            |
| 126     | D. de Florian et al., hep-ph/9711440 LO set 3                                                                            |
| 127     | D. de Florian et al., hep-ph/9711440 NLO set 1                                                                           |
| 128     | D. de Florian et al., hep-ph/9711440 NLO set 2                                                                           |
| 129     | D. de Florian et al., hep-ph/9711440 NLO set 3                                                                           |
| 130     | Fake sample: unpolarized Gehrmann et al hep-ph/9512406 with Delta u(x) = 0.5 \* u(x) and Delta d(x) = 0.                 |
| 131     | Fake sample: unpolarized Gehrmann et al hep-ph/9512406 with Delta d(x) = 0.5 \* d(x) and Delta u(x) = 0.                 |
| 132     | fit routine. (Is outside the official code.)                                                                             |
| 133     | CTEQ4LQ . UNPOL: Low Q2 parametrization is the only one used here. POL: BOGUS, du=0.5\* u(x) dd=-0.3\*d(x) 0.0 all else  |
| 137     | MRS low Q2                                                                                                               |
| 144     | grsv2000 hep-ph/0011215 LO standard scenario                                                                             |
| 145     | grsv2000 hep-ph/0011215 LO valence scenario                                                                              |
| 146     | grsv2000 hep-ph/0011215 NLO standard scenario                                                                            |
| 147     | grsv2000 hep-ph/0011215 NLO valence scenario                                                                             |
{:.table-bordered}
{:.table-striped}

<br />

#### Table 2: Unpolarized parton distributions

| LST(15) | Unpolarized parton distribution function                                                                                   |
| ------- | ------------------------------------------------------------------------------------------------------------------------ |
| 150 | cteq5l LO                       |
| 151 | cteq5m NLO MSBAR                |
| 152 | cteq5m1 NLO MSBAR (update)      |
| 161 | mrs99 cor01 central gluon, a\_s |
| 162 | mrs99 cor02 higher gluon        |
| 163 | mrs99 cor03 lower gluon         |
| 164 | mrs99 cor04 lower a\_s          |
| 165 | mrs99 cor05 higher a\_s         |
| 166 | mrs99 cor06 quarks up           |
| 167 | mrs99 cor07 quarks down         |
| 168 | mrs99 cor08 strange up          |
| 169 | mrs99 cor09 strange down        |
| 170 | mrs99 cor10 charm up            |
| 171 | mrs99 cor11 charm down          |
| 172 | mrs99 cor12 larger d/u          |
| 173 | cteq6l LO                       |
| 174 | cteq6d DIS NLO                  |
| 175 | cteq6m NLO MSBAR                |
{:.table-bordered}
{:.table-striped}
