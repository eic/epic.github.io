---
title: elSpectro
name: elspectro
category: elspectro
layout: default
---

{% include layouts/title.md %}

* TOC
{:toc}


#### About

Event Generator framework for incorporating Spectroscopy into electro / photoproduction reactions.

Currently hosted on [GitHub](https://github.com/dglazier/elSpectro),
please refer to README there for the most up-to-date information.

Contacts
* [Derek Glazier](https://github.com/dglazier)

#### Running
To run the preinstalled package on cvmfs (natively at the labs
or via [singularity](/software/eicsmear_generators_singularity.html)),
first prepare the environment:
```
setenv LD_LIBRARY_PATH ${EICDIRECTORY}/gcc-8.3/:${LD_LIBRARY_PATH}
source /cvmfs/sphenix.sdcc.bnl.gov/gcc-8.3/opt/sphenix/core/gcc/8.3.0.1-0a5ad/x86_64-centos7/setup.csh
source /cvmfs/sphenix.sdcc.bnl.gov/gcc-8.3/opt/sphenix/core/binutils/2.30-e5b21/x86_64-centos7/setup.csh
source /cvmfs/sphenix.sdcc.bnl.gov/gcc-8.3/opt/sphenix/core/root-6.22.02/bin/thisroot.csh
setenv $PATH $EICDIRECTORY/gcc-8.3/bin:${PATH}
```
This step is necessary because elSpectro requires a newer gcc version than the
CentOS7 / SL7 default.

Then in an appropriate directory, for example do:
```
cp $EICDIRECTORY/PACKAGES/elSpectro/examples/ComparePhaseSpaceto2.C .
elspectro ComparePhaseSpaceto2.C
```

#### More examples
All are found in the ```examples``` sub-directory
(```$EICDIRECTORY/PACKAGES/elSpectro/examples``` on cvmfs).


##### Examples comparing to simple weighting of TGenPhaseSpace

1) Decay a rho meson to 2 pi
```
elspectro ComparePhaseSpaceto2.C
```

2) Decay g+p -> rho(pi+,pi-) p
```
elspectro ComparePhaseSpaceto3Rho.C
```

3) Decay g+p -> X(rho(pi+,pi-) , phi(K+,K-) ) p
```
elspectro ComparePhaseSpaceto5RhoPhi.C
```

##### Examples of ElectroProduction

1) EIC e + p -> e' rho(pi+,pi-) p
```
elspectro RunRhoProton.C
hW.Draw()
hQ2.Draw()
hRhoM.Draw()
```

##### Examples of ElectroProduction of Jpac amplitudes

1) e + p -> e' X (Jpsi (e+e-)rho(pi+,pi-)) p
```
elspectro 'EIC_JPAC_X3872.C("high",5,41,1E33,10)'
```

Which will run with 5GeV e- energy, 41 GeV proton, Luminosity=10^33 for 10 days

Or with diagnostic histgrams
```
elspectro 'EIC_JPAC_X3872_Hists.C("high",5,41,1E33,10)'
```

The first argument can be "high" or "low" giving different parameterisations.

To set luminosity and days change last 2 arguments, e.g. for luminosity 10^33 and 25 days, e- energy 100 and p energy 100 with high energy paramterisation :
```
elspectro 'EIC_JPAC_X3872.C("high",100,100,1E33,25)'
```

To just run a fixed number of events leave last argument 0 and nLumi=number of events
```
elspectro 'EIC_JPAC_X3872.C("high",100,100,1E4)'
```

2) e + p -> e' Z(3900) (Jpsi (e+e-) pi+) n

To run with luminosity 10^33 for 25 days
```
elspectro 'EIC_JPAC_nZc_Hists.C("low",5,41,1E33,25)'
```

or to just run 1000 events
```
elspectro 'EIC_JPAC_nZc_Hists.C("low",5,41,1000)'
```

##### Examples of MesonEx Quasi-real PhotoProduction

Note forward tagger acceptance can be included with lines like
```
production->SetLimitTarRest_eThmin(1.5*TMath::DegToRad());
production->SetLimitTarRest_eThmax(5.5*TMath::DegToRad());
production->SetLimitTarRest_ePmin(0.4);
production->SetLimitTarRest_ePmax(6);
```


1) e + p -> e' X (pi+pi-) p
```
elspectro MesonEx_p2pi.C
```

2) e + p -> e' X (pi+pi+pi-) n
```
elspectro MesonEx_n3pi.C
```

3)  e + p -> e' P_c -> Jpsi(e+e-) p
```
elspectro MesonEx_JpsiPenta.C
```

Also just does phase space Jpsi, which does not need jpacPhoto, see code for details


#### Installation from scratch

##### Prerequisites

* ROOT 6 with Minuit2 support
* We require jpacPhot but this is included as a submodule. You may link to your own version of you prefer.
* c++ v5.0 or greater

##### Clone
```
git clone --recurse-submodules https://github.com/dglazier/elSpectro
cd elSpectro
```

##### Set Environment
```
setenv ELSPECTRO /path/to/elSpectro (or $PWD)
setenv PATH ${PATH}:${ELSPECTRO}/bin
```

##### Build with cmake
```
mkdir build; cd build;
cmake ../
cmake --build . --target install
```

In the first cmake step, you can specify installation locations with
```
cmake -DCMAKE_INSTALL_BINDIR=/some/where/bin -DCMAKE_INSTALL_LIBDIR=/some/where/lib
```
