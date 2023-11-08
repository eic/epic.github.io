{% assign what=include.what %}
{% assign owners = site.data.people | where: "name", what.owner %}
{% assign owner = owners[0] %}

<tr>
  <td>{{ what.full }}</td>
  <td><a href="{{ what.repo }}" target="_blank">{{ what.repo }}</a></td>
  <td>{{ owner.full }}</td>
</tr>
