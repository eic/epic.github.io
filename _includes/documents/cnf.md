{%- assign c4y=site.data.keywords | where_exp: "item", "item.year==include.year" | where_exp: "item", "item.category=='conference'" | sort: "name" -%}

<table width="80%" border="1">
<tr><th class="text-center">Keyword</th><th class="text-center">Conference Description</th></tr>
{% for item in c4y %}
  <tr>
    <td width="20%"><nobr><a href="{{ site.zenodo_query_base }}{{ item.name }}" target="_blank">{{ item.name }}</a></nobr></td>
    <td width="80%">{{ item.description}}</td>
  </tr>
{% endfor %}
</table>
