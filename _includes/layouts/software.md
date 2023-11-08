{% include layouts/title.md %}

{% assign items=site.data.software | where: "category", include.category %}

{% assign main=items | where: "type", "main" %}
{% assign tutorials=items | where: "type", "tutorial" %}

{% if main.size>0 %}
{% assign item=main[0] %}
#### {{ item.full }}
<table width="100%">
{% include layouts/software_item.md what=item %}
</table>
{% endif %}


{% if tutorials.size>0 %}
#### Tutorials
<table width="100%">

{% for tutorial in tutorials %}
{% include layouts/software_item.md what=tutorial %}
{% endfor %}

</table>
{% endif %}
