{% if include.width %}
{% assign width=include.width %}
{% else %}
{% assign width='100%' %}
{% endif %}

<table border="1" width="{{ width }}">
  {% assign headers = include.headers | split: ", " %}
  <tr>
    {% for header in headers %}
    <th>{{ site.tablepadding }}{{ header }}</th>
    {% endfor %}
  </tr>
  {% for row in include.rows %}
  <tr>
    {% assign columns = row | split: ", " %}
    {% for item in columns %}
    <td>{{ site.tablepadding }}{{ item }}</td>
    {% endfor %}
  </tr>
  {% endfor %}
  
</table>
