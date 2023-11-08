{%- if include.title -%}
{%- assign tag=include.title -%}
{%- else -%}
{%- if include.tag -%}
{%- assign tag=include.tag -%}
{%- else -%}
{%- assign tag=include.name -%}
{%- endif -%}
{%- endif -%}

{%- assign found_items=site.data.links | where: "name", include.name -%}
{%- if include.category %}
{%- assign found_items=found_items | where: "category", include.category | compact -%}
{%- endif -%}
{%- assign found_link= found_items | map: "url" | first -%}
{%- if found_link -%}
{%- if include.category -%}
* [{{ tag }}]({{ found_link }}){:target="_blank"}
{%- else -%}
{% if include.html %}
<a href="{{ found_link }}" target="_blank">{{ tag }}&nbsp;<img src="{{ site.external_icon | relative_url }}" height="16" width="16"></a>
{%- else -%}
[{{ tag }}]({{ found_link }}){:target="_blank"}
{%- endif -%}
{%- endif -%}
{%- endif -%}
