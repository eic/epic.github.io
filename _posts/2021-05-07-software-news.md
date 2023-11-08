---
title: Software News May 2021
author: Markus Diefenthaler
layout: default
symbol: glyphicon-calendar
until: 2021-12-31
---
<p/>

{% include images/image.md name='news_banner' width='800' %}

The Software Working Group (SWG) is open to all members of the EICUG
to work on EICUG-related software tasks. The SWG regularly
communicates via its [mailing list](mailto:eicug-software@eicug.org), and organizes regular [online and
in-person meetings](https://indico.bnl.gov/category/301/) that enable
broad and active participation from within the EICUG.

The SWG has participated in the call for Expressions of Interest (EoI)
and has carried its [EoI for
Software](https://eic.github.io/activities/eoi.html) forward as a
living document that will evolve toward a work plan for the SWG,
setting priorities for the next years and goals for the next decade.

Furthermore, the SWG strongly supports the simulation efforts for the
collaboration proposals for detectors at the EIC. Part of the Software
EoI is eAST, a project on a common Geant4 simulation toolkit. The
deliverables for Project eAST will include the physics lists and the
test beam based simulation tuning and validation for the detector
collaboration proposals this year.  Additionally, it will provide a
possible path for convergence of detector simulation tools for the
following year.

## Table of contents

* [Updates from the Software Working Group](#swg)
    * [EIC School of Software & Computing](#tutorials)
    * [EIC Software Bundle](#bundle)
    * [Project eAST](#east)
* [Updates from the Proto-Collaborations](#proto)
    * [ECCE](#ecce)
    * [EIC@IP6](#eicatip6)

---

## Updates from the Software Working Group {#swg}

### EIC School of Software & Computing {#tutorials}
The SWG will continue hosting the EIC Software tutorials. Please be sure to add your suggestions for topics that we should cover in our next tutorials in a [Google document](https://docs.google.com/document/d/15XgfEeOH1PnRmvPDsobX92dmEfusvibIZogvCR-JcNg/edit?usp=sharing) (anyone can edit). Previous tutorials are available on the [YouTube channel of the EICUG](https://www.youtube.com/channel/UCXc9WfDKdlLXoZMGrotkf7w). 

### EIC Software Bundle {#bundle}
The EIC Software bundle contains EIC-SMEAR and various MC event generators. It is centrally installed and distributed via CVMFS (`/cvmfs/eic.opensciencegrid.org/x8664_sl7/MCEG/`).  The *dev* version of the EIC Software bundle has been updated to *EIC2021a*, including many updates to the environment and the event generators. Notable changes are: 
* The **BeAGLE** event generator has been updated to version 1.1. Please note: This update includes major changes and fixes, older versions should no longer be used.
* [**elSpectro**](https://github.com/dglazier/elSpectro), an event generator for spectroscopy, has been included. 
* **Pythia 6** The PYTHIA-RAD-CORR directory now contains a subdirectory named STEER-FILES-Official containing the recommended best configuration cards for EIC simulations. A [fix to improve kinematic approximations](https://gitlab.com/eic/mceg/PYTHIA-RAD-CORR/-/commit/b3ceac3ac63647906ae1219a80ca6be6bbadc877) was added. The executable for Pythia 6 has been renamed to pythia-eic (with a symlink to pythiaeRHIC for backward compatibility). 
* [**TOPEG**](https://gitlab.in2p3.fr/dupre/nopeg), an event generator for nuclear DVCS, has been added. 

### Project eAST {#east}

{% include images/image.md name='east_logo' width='400' %}

The SWG is launching a community-wide effort on next-generation simulations. Project eAST (eA Simulation Toolkit) will be led by Makoto Asai of SLAC, Geant4 project leader and deep technical expert for more than 20 years, and will build on the work done in existing detector simulations for the EIC. To ease leveraging new,  rapidly evolving computing technologies, the SWG will implement a common integrated approach for fast and full detector simulations in Geant4 with a plug-and-play modular approach. An [overview of the project](https://docs.google.com/presentation/d/1i3_MG26J93OqOuZx8MJY_btmpGpuxPRCOfdJHAHFPwY/edit?usp=sharing) and a [proposal with comments by the community](https://docs.google.com/document/d/1-EduKk_hCUr2lnKZFyqCMyzQBaM4ABRTRfFeOrEtWo8/edit?usp=sharing) is available.

---

## Updates from the Proto-Collaborations {#proto}

### ECCE {#ecce}
ECCE has started to develop a [GitHub repository space for simulation, reconstruction and analysis development](https://github.com/ECCE-EIC) in response to the call for detector collaboration proposals. Any and all contributions to the ECCE repositories, as well as the upstream Fun4All and EICUG repositories, are welcome. 

On the [ECCE software website](https://ecce-eic.github.io/), you will find dedicated repositories for tutorials, developing analysis code, new subdetectors, and running full Geant4 simulations. Additionally, there is a Singularity container that allows for any user to work on their local laptop, with minimal setup required. 
 
Recently, a workshop was held to demonstrate how to use several software tools for analysis development. The tutorials at the workshop were recorded and are available on the following Indico page: [https://indico.bnl.gov/event/11112/](https://indico.bnl.gov/event/11112/)
 
If you are interested in contributing, please don’t hesitate to check out the available resources or reach out to one of the speakers for more information on their talk or other software-related questions. If you have any additional questions, please contact [Joe Osborn](mailto:osbornjd@ornl.gov), the liaison between ECCE and the SWG.  

### EIC@IP6 {#eicatip6}

The software group of EIC@IP6 is actively developing tools for a full simulation chain based on a modern modular approach: 

* [Integration of the reference detector in DD4HEP](https://eicweb.phy.anl.gov/EIC/detectors/reference_detector) is very advanced and will be completed soon.
* Factored out [implementation of the interaction region](https://eicweb.phy.anl.gov/EIC/detectors/ip6) and [accelerator](https://eicweb.phy.anl.gov/EIC/detectors/accelerator).
* Different tracking/vertexing options for ACTS are being prepared. 
* The current approach uses GitLab CI for automized benchmarks/tests, both on dedicated servers and dispatched on a SLURM-based HPC system. Going forward, we will integrate this with a middleware solution, and use Rucio for distributed data management. In the short term, we will manually use S3 and/or Globus to keep the different sites in sync.

Meanwhile, detector studies will progress using currently available packages. On May 6, we will begin bi-weekly meetings for the software group. If you have any additional questions, please contact [Andrea Bressan](mailto:andrea.bressan@ts.infn.it), the liaison between EIC@IP6 and the SWG.  

---

### About the Software Working Group

Please see our [website](https://eic.github.io) for more information about the Software Working Group and engage in the discussion on our [mailing list](mailto:eicug-software@eicug.org). For questions about the SWG, please [contact the conveners](mailto:eicug-software-conveners@eicug.org) (Andrea Bressan (Trieste), Markus Diefenthaler (JLab), and Torre Wenaus (BNL).

---
