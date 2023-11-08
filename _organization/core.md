---
title: Core Group
name: core
layout: default
---

{% include layouts/title.md %}

<table width="100%">
{% assign people=site.data.people | sort: "name" %}
{% for who in people %}
<tr>
<td>{{ who.full }}</td>
<td><a href="mailto:{{ who.email }}">{{ who.email }}</a></td>
</tr>
{% endfor %}
