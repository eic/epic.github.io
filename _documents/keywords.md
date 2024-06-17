---
title: ePIC Keywords
name: keywords
layout: default
years:
- 2024
---
{% include layouts/title.md %}


This page is under development.

{% for year in page.years %}
{% assign keys=site.data.keywords | sort: "name" %}

<table width="50%" border="1">
<tr><td>Keyword</td><td>Description</td></tr>
{% for item in keys %}
  <tr>
    <td width="50%"><nobr><a href="{{ site.zenodo_query_base }}{{ item.name }}" target="_blank">{{ item.name }}</a></nobr></td>
    <td width="50%">{{ item.description}}</td>
  </tr>
{% endfor %}
</table>

{% endfor %}
