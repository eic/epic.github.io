{% if include.table.width %}
{% assign width=include.table.width %}
{% else %}
{% assign width='100%' %}
{% endif %}

<table border="1" width="{{ width }}">
  {% if include.table.columns %}
  {% for col in include.table.columns %}
  <col width="{{ col }}"/>
  {% endfor %}
  {% endif %}
  
  <tr>
    {% for header in include.table.headers %}
    <th>{{ site.tablepadding }}{{ header }}</th>
    {% endfor %}
  </tr>
  {% for row in include.table.rows %}
  <tr>
    {% for item in row %}
    <td>{{ site.tablepadding }}{{ item }}</td>
    {% endfor %}
  </tr>
  {% endfor %}
  
</table>
