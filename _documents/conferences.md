---
title: Conferences with ePIC participation
name: conferences
layout: default
years:
- 2024
---
{% include layouts/title.md %}

The left column in the tables below contains links to the respective conferences' pages.

Conference "contributions" links represent collections of relevant ePIC items on Zenodo.
Please note that some conference uploads may be still pending i.e. not all queries will produce results.

{% for year in page.years %}
{% assign c4y=site.data.keywords | where_exp: "item", "item.year==year" | where_exp: "item", "item.category=='conference'" | sort: "name" %}

<h5>{{ year }}</h5>
<table width="80%" border="1">
{% for conference in c4y %}
  <tr>
    <td width="80%"><nobr><a href="{{ conference.url }}" target="_blank">{{ conference.description }}</a></nobr></td>
    <td width="20%"><nobr><a href="{{ site.zenodo_query_base }}{{ conference.name }}" target="_blank">Contributions</a></nobr></td>
  </tr>
{% endfor %}
</table>

{% endfor %}
