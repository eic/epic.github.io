---
title: Software Working Group
name: swg
layout: default
---

{% include layouts/title.md %}

##### Conveners
{% include people/conveners.md %}
<br/>
Conveners' mailing list: <a href="mailto:eicug-software-conveners@eicug.org">eicug-software-conveners@eicug.org</a>
<hr/>

##### The Group

The Software Working Group includes more than 100 motivated physicists and software professionals with interest in
various aspects of EIC Software and Computing.
<br/><br/>
The Software Group mailing list: <a href="mailto:eicug-software@eicug.org">eicug-software@eicug.org</a>
<hr/>

##### The Charge

The EICUG Software Working Group’s initial focus will be on
simulations of physics processes and detector response to enable
quantitative assessment of measurement capabilities and their physics
impact. This will be pursued in a manner that is accessible,
consistent, and reproducible to the EICUG as a whole.

It will embody simulations of all processes that make up the EIC
science case as articulated in the white paper. The Software working
group is to engage with new major initiatives that aim to further
develop the EIC science case, including for example the upcoming INT
program(s), and is anticipated to play key roles also in the
preparations for the EIC project(s) and its critical decisions. The
working group will build on the considerable progress made within the
EIC Software Consortium (eRD20) and other efforts. The evaluation or
development of experiment-specific technologies, e.g. mass storage,
clusters or other, are outside the initial scope of this working group
until the actual experiment collaborations are formed.

The working group will be open to all members of the EICUG to work on
EICUG related software tasks. It will communicate via its [mailing
list](mailto:eicug-software@eicug.org) and organize regular online and
in-person meetings that enable broad and active participation from
within the EICUG as a whole.
<hr/>
##### The Core Team
The core team consists of EICUG collaborators making frequent contributions to the effort.
{% assign people=site.data.people | sort: "name" %}

<table width="100%">
{% for who in people %}
<tr>
<td>{{ who.full }}</td>
<td><a href="mailto:{{ who.email }}">{{ who.email }}</a></td>
</tr>
{% endfor %}
</table>
<br/>
The Core Team mailing list: <a href="mailto:eicug-software-core@eicug.org">eicug-software-core@eicug.org</a>
<hr/>
