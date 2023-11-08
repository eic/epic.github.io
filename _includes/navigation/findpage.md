{% assign found_page=include.folder | where: "name", include.name | map: "url" | first | relative_url %}
{{ found_page }}
