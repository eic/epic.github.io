{%- assign item = site.data.documents | where: "name", include.name | first -%}
{% comment %}
[{{ item.title }}]({{ item.url }}){:target="_blank"} ({{ item.author }})
{% endcomment %}
{%- assign document_tag=item.title -%}
{%- if include.tag -%}
{%- assign document_tag=include.tag -%}
{%- endif -%}
<a href="{{ item.url }}" target="_blank">{{document_tag}}</a> ({{ item.author }})


