<table width="75%" border="1">

{% for who in site.data.people %}
{% if who.roles contains "convener" %}
<tr>
<td>{{ who.full }}</td>
<td><a href="mailto:{{ who.email }}">{{ who.email }}</a></td>
</tr>
{% endif %}
{% endfor %}
</table>

