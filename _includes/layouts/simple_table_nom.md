
<table class="plain" width="60%">
<tr><th width="30%">Name</th><th>Title</th></tr>
{% for d in include.data %}
<tr><td width="30%">{{ d.name }}</td><td>{{ d.title }}</td></tr>
{% endfor %}
</table>

