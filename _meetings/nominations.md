---
title: ePIC Conference Nominations
description: Nominations of speakers for conferences
name: nominations
layout: default
---

{% include layouts/title.md %}

{% assign nominations=site.data.keywords | where_exp: "item", "item.category=='conference'" | sort: "name" %}

{% for nom in nominations %}
{% if nom.nominations %}
{{ nom.nominations.name }}

{% endif %}
{% endfor %}
