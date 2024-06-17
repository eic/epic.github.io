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
{% for year in page.years %}

##### {{ year }}
{% assign c4y=site.data.keywords | where_exp: "item", "item.year==year" | where_exp: "item", "item.category=='conference'" | sort: "name" %}

<table width="50%" border="1">
<tr><td>Keyword</td><td>Description</td></tr>
{% for item in c4y %}
  <tr>
    <td width="50%"><nobr><a href="{{ site.zenodo_query_base }}{{ item.name }}" target="_blank">{{ item.name }}</a></nobr></td>
    <td width="50%">{{ item.description}}</td>
  </tr>
{% endfor %}
</table>
{% endfor %}


<hr/>
#### Document keywords
{% assign keys=site.data.keywords | where_exp: "item", "item.category=='documents'" | sort: "name" %}
<table width="50%" border="1">
<tr><td>Keyword</td><td>Description</td></tr>
{% for item in keys %}
  <tr>
    <td width="50%"><nobr><a href="{{ site.zenodo_query_base }}{{ item.name }}" target="_blank">{{ item.name }}</a></nobr></td>
    <td width="50%">{{ item.description}}</td>
  </tr>
{% endfor %}
</table>


<hr/>
#### Detector keywords
{% assign keys=site.data.keywords | where_exp: "item", "item.category=='detector'" | sort: "name" %}
<table width="50%" border="1">
<tr><td>Keyword</td><td>Description</td></tr>
{% for item in keys %}
  <tr>
    <td width="50%"><nobr><a href="{{ site.zenodo_query_base }}{{ item.name }}" target="_blank">{{ item.name }}</a></nobr></td>
    <td width="50%">{{ item.description}}</td>
  </tr>
{% endfor %}
</table>
