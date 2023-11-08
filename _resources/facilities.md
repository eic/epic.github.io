---
title: Facilities
description: Resources available at BNL and JLab
name: facilities
layout: default

tables:
  cpu:
    width: 90%
    headers:
      - BNL
      - JLab
    rows:
      - [
      "Dedicated EIC resources: 7 machines with 64 core,
      6&times;64 batch slots = 384 slots and one for interactive use (VMs).
      Each node has dual CPU AMD EPYC 7351.",

      "JLab operates a common cluster. 25k job slots. Delivering ~ 76M core hours per year normalized to dual CPU AMD EPYC 7351."
      ]

      - [
      "1,000 slots allocated from a variety of CPUs (Intel<sup>&reg;</sup> Xeon<sup>&reg;</sup> and AMD EPYC)",
      
      "EIC is currently one of ten projects sharing a 10% fair-share of the cluster.
      So guaranteed 7.6M core hours if the other nine are idle 0.8M if all are busy"
      ]

      - [
      "31k slots available to all experiments that can be dynamically allocated upon priorities (358 M core hours per year).",

      "A further 4MCH/yr dedicated to EIC will be added when demand requires it."
      ]
  wms:
    width: 90%
    headers:
      - BNL
      - JLab
    rows:
      - [
      "OSG Submit host osgsub01.sdcc.bnl.gov",
      "OSG submit host"
      ]
      - [
      "OSG/EIC jobs landing at BNL are purely opportunistic.",
      "OSG infrastructure to land EIC jobs on the JLab cluster. Counts against quota."
      ]
  
  data:
    width: 90%
    headers:
      - BNL
      - JLab
    rows:
      - [
      "<b>/eic/data</b> 6TB NFS mounted (GPFS general user use), 72% full",
      "EIC is allocated 5TB of volatile and 5TB of cache neither of which are close to the limit.
      The total pool at JLab is 5PB.
      Volatile and Cache are auto managed with a deletion policy so allocation can be changed."
      ]
      - [
      "<b>/gpfs02/eic/DATA</b> 400TB NFS mounted (GPFS data use), 53% full",
      ""
      ]
      - [
      "Xrootd/StashCache under preparation",
      "Xrootd/StashCache available"
      ]
      - [
      "1PB pool (S3 or Xrootd)",
      ""
      ]
  
  aux:
    width: 90%
    headers:
      - BNL
      - JLab
    rows:
      - [
      "CernVM-FS Stratum 0 repository server",
      "CernVM-FS Stratum 1 repository server"
      ]
      - [
      "Globus for data transfer",
      "Globus for data transfer"
      ]
      - [
      "BNLBox data export possible ([documentation](https://eic.github.io/resources/storage.html))",
      ""
      ]
  
---
{% include layouts/title.md %}

{{ page.tablewidth }}

BNL and JLab provide computing resources for the worldwide EIC community:

* TOC
{:toc}

##### CPU
{% include layouts/big_table.md table=page.tables.cpu %}

<br/>
##### Workload Management
{% include layouts/big_table.md table=page.tables.wms %}

<br/>
##### Data Storage
{% include layouts/big_table.md table=page.tables.data %}

<br/>
##### Auxiliary Data Services
{% include layouts/big_table.md table=page.tables.aux %}
