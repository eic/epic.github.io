{%- if include.category=='proposed' -%}
{%- assign keys=site.data.keywords | where_exp: "item", "item.category==include.category" -%}
{%- else -%}
{%- assign keys=site.data.keywords | where_exp: "item", "item.category==include.category" | sort: "name" -%}
{%- endif -%}

<table width="70%" border="1">
<tr><th class="text-center">Keyword</th><th class="text-center">Description</th></tr>
{% for item in keys %}
  <tr>
    {%- if item.category=='proposed' or item.upload==false -%}
    <td width="20%">{{ item.name }}</td>
    {%- else -%}
    <td width="20%"><nobr><a href="{{ site.zenodo_query_base }}{{ item.name }}" target="_blank">{{ item.name }}</a></nobr></td>
    {%- endif -%}
    <td width="80%">{{ item.description}}</td>
  </tr>
{% endfor %}
</table>
