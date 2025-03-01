---
title: Conference Keywords
name: confkw
layout: default
years:
- 2025
- 2024
- 2023
---
{% include layouts/title.md %}

* [Back to the main keyword page](/documents/keywords.html)
* [A dedicated conference page with links to the conferences' websites](/documents/conferences.html)

---

Conference keywords represent links to collections of relevant ePIC items on Zenodo.
Materials relevant to a given conference, uploaded to Zenodo, should always be tagged
with the corresponsing keyword for that conference.

Please note that some uploads may be still pending i.e. not all queries will produce results.
For easy access, the conferences are grouped by the year.


{% comment %}

{% for year in page.years %}
##### {{ year }}

{% include documents/cnf.md year=year %}

<br/>

---

{% endfor %}

{% endcomment %}