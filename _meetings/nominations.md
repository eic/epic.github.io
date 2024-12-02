---
title: ePIC Conference Nominations
description: Nominations of speakers for conferences
name: nominations
layout: default
---

{% include layouts/title.md %}

{% assign conferences=site.data.keywords | where_exp: "item", "item.category=='conference'" | sort: "name" %}

{% for conf in conferences %}
{% if conf.nominations %}

##### {{ conf.description }}

{% include layouts/simple_table_nom.md data=conf.nominations %}

---

{% endif %}
{% endfor %}


{% comment %}
{% for nom in conf.nominations %}
{{ nom.name }}
{% endfor %}
{% endcomment %}