
<table class="plain">
{% for c in include.people %}
<tr><td>&nbsp;</td><td>&#x25cf;&nbsp; {{ c.name }}</td><td><a href="mailto:{{ c.email }}">{{ c.email }}</a></td></tr>
{% endfor %}
</table>

