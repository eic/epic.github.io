---
title: Running ESCalate on HPC clusters
name: escalate_singularity_1
category: escalate
layout: default
---

{% include layouts/title.md %}

- [Singularity images on CVMFS](#singularity-images-on-cvmfs)
- [Building a local singularity container](#building-a-local-singularity-container)
- [Running ESCalate on HPC clusters](#running-escalate-on-hpc-clusters)
- [Running scripts](#running-scripts-inside-the-singularity-container)
- [Running programs](#running-programs-inside-the-singularity-container)
- [Scripts with multiple commands](#scripts-with-multiple-commands-inside-the-singularity-container)
- [Scheduling batch jobs](#-scheduling-batch-jobs-with-the-singularity-container)
- [Advanced ways of running](#advanced-ways-of-running)



<br>

#### Singularity images on CVMFS

To run the ESCalate docker container on HPC clusters, we use singularity. This does not require super-user privileges.


Our [docker image](https://gitlab.com/eic/containers) is being automatically converted to singularity on any update. 

This singularity images available at **BNL** and **JLab** and anywhere where one has has access to the EIC repository 
on the cvmfs filesystem (at `/cvmfs/eic.opensciencegrid.org`).

```
/cvmfs/singularity.opensciencegrid.org/electronioncollider/escalate\:latest
```

**(!)** All instructions below works if you put the above link there instead of `electronioncollider/escalate:latest`


<br>

#### Building a local singularity container

Since the docker container is quite large, you will want to build the singularity image in a directory where you have plenty of space (e.g. `/volatile` at Jefferson Lab). You do *not* want to save this image to your home directory!

```bash
mkdir -p /volatile/eic/$USER/singularity
cd /volatile/eic/$USER/singularity
module load singularity
export SINGULARITY_CACHEDIR="."
singularity build --sandbox electronioncollider/escalate:latest docker://electronioncollider/escalate:latest
```

Setting the singularity cache directory `SINGULARITY_CACHEDIR` prevents singularity from filling up a hidden directory in your home directory with 10 GB of docker container layers.

While building the container, singularity may complain a bit about `EPERMS` and `setxattr`, but you can ignore this (the drawback of doing this without super-user privileges).

After lunch, you should have a sandbox container (i.e. a directory which encapsulates an entire operating system) at `/volatile/eic/$USER/singularity/electronioncollider/escalate:latest/`.

Note: If you aready have pulled the container into docker locally, you can save some downloading time by building the singularity image from the local container with
```bash
singularity build --sandbox electronioncollider/escalate:latest docker-daemon://electronioncollider/escalate:latest
```

Note: As a temporary fix for older kernels < 3.15 (check with `uname -r`) , run the following before using the image:
```bash
strip --remove-section=.note.ABI-tag electronioncollider/escalate:latest/usr/lib/x86_64-linux-gnu/libQt5Core.so.5.12.4
```


<br>

#### Running interactive shells inside the singularity container

You can open a shell in the container with
```bash
singularity shell --cleanenv electronioncollider/escalate:latest
```
Inside the singularity container your home directory will be accessible (and possibly some other shared drives), allowing for easy interaction with files on the host system but software inside the container.

The `--cleanenv` or `-e` flag indicates that environment variables should not be kept (this ensures that, for example, `$ROOTSYS` is not pointing to the location in the host system). This does, however, also sever the x11 forwarding that allows graphics windows to appear. While at first it may be advisable to use `-e`, you may find that everything works without it.

In the shell you will not have the shell environment setup until you run
```bash
source /etc/profile
```
Now you should be able to run any commands you would run inside a terminal in the JupyterLab interface, e.g. `root -l` should open a ROOT session.

We can now, for example, run `dire` and generate some pythia8 events using the following command:
```bash
dire --nevents 50 --setting "WeakSingleBoson:ffbar2gmZ = on"
```
This will generate files in the current directory (300 KB, 30 seconds).

You can exit the shell again with `exit` or Ctrl-D.


<br>

#### Running scripts inside the singularity container

Instead of interactively logging in and running commands, we can also create a script and run that from the host system command line. If we have an executable script `script.sh` with content
```bash
#!/bin/bash

dire --nevents 50 --setting "WeakSingleBoson:ffbar2gmZ = on"
```
then we can run this directly as
```bash
singularity run electronioncollider/escalate:latest ./script.sh
```
We need the `./` because that is what we would have typed inside the container because the current directory is (likely) not in the search path.

Note: You may encounter a warning from `tini`, the master process inside the container (similar to `init`, see what they did there?). You can remove the warning by defining an empty environment variable with `export TINI_SUBREAPER=""`.


<br>

#### Running programs inside the singularity container

Now, if we can run scripts directly wtih `singularity run`, then why not run `dire` directly, you ask? Indeed, we can also run
```bash
singularity run electronioncollider/escalate:latest dire --nevents 50 --setting "WeakSingleBoson:ffbar2gmZ = on"
```
Note: since `dire` is in the search path, we do not need to specify the full path. Arguments can be passed on as usual.

This can be even more simplified if we just define an environment variable `$ESC` with
```bash
export ESC="singularity run `pwd`/electronioncollider/escalate:latest"
```
where the `pwd` is to ensure that an absolute path is used.

We can now prepend this to any command we want to run inside the container, no matter which directory we are in:
```bash
$ESC dire --nevents 50 --setting "WeakSingleBoson:ffbar2gmZ = on"
```


<br>

#### Scripts with multiple commands inside the singularity container

To be able to run batch jobs, we want to submit job scripts. For example, we can create a script with the following content (anywhere you want):
```bash
#!/bin/bash

cd $HOME

export ESC="singularity run /volatile/eic/$USER/singularity/electronioncollider/escalate:latest"

$ESC dire --nevents 50 --setting "WeakSingleBoson:ffbar2gmZ = on"
```
This will use the container to run the `dire` command specified and write the output to your home directory.


<br>

#### Scheduling batch jobs with the singularity container

We can extend the previous script to turn it into a SLURM submission script, `job.sh`:
```bash
#!/bin/bash

#SBATCH --time=0:05:00
#SBATCH --mem-per-cpu=512
#SBATCH --account=eic
#SBATCH --partition=production

cd $HOME

export ESC="singularity run /volatile/eic/$USER/singularity/electronioncollider/escalate:latest"

$ESC dire --nevents 50 --setting "WeakSingleBoson:ffbar2gmZ = on"
```
and submit this with `sbatch`.


<br>

#### Advanced ways of running

One could create a python `fast.py` for a fast simulation (using eic-smear) like this:
```python
from pyjano.jana import Jana

Jana().plugin('beagle_reader')\
      .plugin('open_charm')\
      .plugin('eic_smear', detector='jleic')\
      .plugin('jana', nevents=20000, output='hepmc_sm.root')\
      .source('beagle_eD.txt')\
      .run()
```
and run
```bash
wget https://gitlab.com/eic/escalate/workspace/-/raw/master/data/beagle_eD.txt
$ESC python3 fast.py
```

Or for a full simulation, create `full.py` with:
```python
from g4epy import Geant4Eic
g4e = Geant4Eic()\
      .source('beagle_eD.txt')\
      .output('hello')\
      .beam_on(200)\
      .run()
```
and run
```bash
wget https://gitlab.com/eic/escalate/workspace/-/raw/master/data/beagle_eD.txt
$ESC python3 full.py
```

<br>
