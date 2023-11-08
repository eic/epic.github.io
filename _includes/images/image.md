{%- assign image=site.data.gallery | where: 'name', include.name | first -%}
{%- assign path=image.path | relative_url -%}
<img src="{{ path }}" width="{{ include.width }}"/>
