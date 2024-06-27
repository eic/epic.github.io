---
title: ePIC Keywords
name: keywords
layout: default
years:
- 2024
---
{% include layouts/title.md %}


This page is under development.

#### Conferences

Conference keywords represent links to collections of relevant ePIC items on Zenodo.
Please note that some uploads may be still pending i.e. not all queries will produce results.
{% for year in page.years %}
##### {{ year }}

{% include documents/cnf.md year=year %}
{% endfor %}

<hr/>
#### Document keywords
{% include documents/kw.md category='documents' %}

<hr/>
#### Detector keywords
{% include documents/kw.md category='detector' %}

<br/>

---