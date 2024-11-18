---
title: Conferences and Meetings with ePIC participation
name: conferences
layout: default
years:
- 2025
- 2024
- 2023
---
{% include layouts/title.md %}

##### Policies and Procedures

* The Conference and Talk Committee (CTC) is responsible for the oversight and management of all oral and poster presentations given at scientific conferences on behalf of the Collaboration. 
For contact information and more detail, please see [the committee web page](/collaboration/committees.html){:target="_blank"}.
* The [ePIC Conference and Talks Policy](https://zenodo.org/records/14052729){:target="_blank"} was adopted by Council on November 7, 2024 (restricted to members).

{{ site.hr }}

##### List of Events

* The left column in the tables below contains links to the respective conferences' pages.
* The right column contains official ePIC keywords assigned to each conference.
Keywords rendered as clickable links will list the ePIC items on archived on Zenodo, related to the conference (or meeting).
* Please note that some conference uploads (especially for future conferences) may be still pending i.e. not all queries will produce results.
* For easy access, the conferences are grouped by the year.

{{ site.hr }}

{% for year in page.years %}
{% assign c4y=site.data.keywords | where_exp: "item", "item.year==year" | where_exp: "item", "item.category=='conference'" | sort: "name" %}

<h5>{{ year }}</h5>
<table width="80%" border="1">
{% for conference in c4y %}
  <tr>
    <td width="80%"><nobr><a href="{{ conference.url }}" target="_blank">{{ conference.description }}</a></nobr></td>
    {% if conference.upload != false %}
    <td width="20%"><nobr><a href="{{ site.zenodo_query_base }}{{ conference.name }}" target="_blank">{{ conference.name }}</a></nobr></td>
    {% else %}
    <td width="20%"><nobr>{{ conference.name }}</nobr></td>
    {% endif %}
  </tr>
{% endfor %}
</table>

<br/>

---

{% endfor %}
