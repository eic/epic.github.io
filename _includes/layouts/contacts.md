
<table class="plain" width="60%">
{% for c in include.people %}
<tr><td>&#x25cf; {{ c.name }}</td><td><a href="mailto:{{ c.email }}">{{ c.email }}</a></td></tr>
{% endfor %}
</table>

