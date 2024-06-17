---
title: Conferences with ePIC participation
name: conferences
layout: default
years:
- 2024
---
{% include layouts/title.md %}

{% for year in page.years %}
{% assign c4y=site.data.keywords | where_exp: "item", "item.year==year" | where_exp: "item", "item.category=='conference'" | sort: "name" %}

<h5>{{ year }}</h5>
<table width="50%" border="1">
{% for conference in c4y %}
  <tr>
    <td width="50%"><nobr><a href="{{ conference.url }}" target="_blank">{{ conference.description }}</a></nobr></td>
    <td width="50%"><nobr><a href="{{ site.zenodo_query_base }}{{ conference.name }}" target="_blank">Contributions</a></nobr></td>
  </tr>
{% endfor %}
</table>

{% endfor %}
