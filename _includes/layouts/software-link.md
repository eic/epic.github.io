{% assign items=site.data.software | where: "category", include.category %}

{% assign main=items | where: "type", "main" %}

{% if main.size>0 %}
{% assign item=main[0] %}
  <h6><a href="{{ item.repo }}" target="_blank">{{ item.repo }}</a></h6>
{% endif %}

