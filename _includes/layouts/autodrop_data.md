{% case include.what %}

{% when "software" %}    {% assign theCollection=site.software %}    {% assign icon=site.software_icon %}
{% when "resources" %}   {% assign theCollection=site.resources %}   {% assign icon=site.resources_icon %}
{% when "activities" %}  {% assign theCollection=site.activities %}  {% assign icon=site.activities_icon %}
{% when "organization" %}{% assign theCollection=site.organization %}{% assign icon=site.organization_icon %}
{% when "documentation" %}{% assign theCollection=site.documentation %}{% assign icon=site.documentation_icon %}
{% when "about" %}       {% assign theCollection=site.about %}            {% assign icon=site.about_icon %}

{% endcase %}

{% assign the_menu = site.data.menus | where: "name", include.what | first %}

<li class="nav-item dropdown px-2">
{% if icon.size > 0 %}
<a class="nav-link dropdown-toggle"  href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" style="color: #fff;">{{ the_menu.full }}&nbsp;&nbsp;<img src="{{ icon | relative_url }}" height="16" width="16"></a>
{% else %}
<a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" style="color: #fff;">{{ the_menu.full }}</a>
{% endif %}

<div class="dropdown-menu" aria-labelledby="navbarDropdown">

{% for submenu in the_menu.submenus %}
{% if submenu.exclude %}{% continue %}{% endif %}

{% if submenu.div %}<div class="dropdown-divider"></div>{% endif %}

{% if submenu.label %}<div class="dropdown-item" style="color: #fff; background-color: #888;">{{ submenu.full }}</div>{% continue %}{% endif %}


{% if submenu.link %}
{% assign theLink=submenu.link %}
<a class="dropdown-item" href="{{ theLink }}" {{ site.blank }}>{{ submenu.full }}&nbsp;<img src="{{ site.external_icon | relative_url }}" height="12" width="12"></a>
{% else %}

{% assign item=theCollection | where: "name", submenu.name | first %}
{% assign theLink=item.url | relative_url %}

{% assign experiment=theCollection | where: "name", submenu.name | map: "url" | first | relative_url %}
<a class="dropdown-item" href="{{ theLink }}">{{ submenu.full }}</a>

{% endif %}

{% endfor %}

</div>
</li>
