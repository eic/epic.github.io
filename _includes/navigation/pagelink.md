{%- assign tag=include.name -%}
{%- if include.tag -%}
{%- assign tag=include.tag -%}
{%- endif -%}
{%- assign found_page=include.folder | where: "name", include.name | map: "url" | first | relative_url -%}
[{{ tag }}]({{ found_page }})
