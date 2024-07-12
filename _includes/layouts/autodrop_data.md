{% case {{include.what}} %}

{% when "public" %}      {% assign theCollection=site.public %}    {% assign icon=site.public_icon %}
{% when "physics" %}     {% assign theCollection=site.physics %}   {% assign icon=site.physics_icon %}
{% when "detector" %}    {% assign theCollection=site.detector %}  {% assign icon=site.detector_icon %}
{% when "sc" %}          {% assign theCollection=site.sc %}        {% assign icon=site.software_icon %}
{% when "meetings" %}    {% assign theCollection=site.meetings %}  {% assign icon=site.meeting_icon %}
{% when "documents" %}   {% assign theCollection=site.documents %} {% assign icon=site.literature_icon %}
{% when "news" %}        {% assign theCollection=site.news %}      {% assign icon=site.news_icon %}


{% when "collaboration" %}    {% assign theCollection=site.collaboration %}  {% assign icon=site.organization_icon %}

{% endcase %}

{% assign the_menu = site.data.menus | where: "name", include.what | first %}


{% if include.public == "yes" %}
{% assign the_menu = site.data.menus_public | where: "name", include.what | first %}
{% endif %}


<li class="nav-item dropdown px-2">

{% if include.what=="internal" %}
{% assign internalLink=include.link | relative_url %}
<a class="nav-link"  href="{{internalLink}}" id="navbarDropdown"  style="color: #fff;">For Collaborators</a>
{% else %}
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
{% endif %}
</li>
