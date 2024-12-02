# The ePIC website

## About
This is the future home of the ePIC collaboration website. It is in early stages of development.
Suggestions are welcome.

## Current work items

Fill out the required pages. Please contact T.Ullrich for details.


### New features

WG descriptions are now kept in the file _wg.yml_ in the *_data_* folder

### Content recently updated:
* Committees -- _Conferences and Talks_ updated
* Physics
   * _Semi-Inclusive_
   * _Exclusive and Diffraction_
   * _Jets and HF_
   * _BSM and EW_

* Keywords (a stub)

### Filtering

Work in progress, examples:
```
{% assign nominations=site.data.keywords | where_exp: "item", "item.category=='conference'" | where_exp: "item", "item.year==2024" %}

{% comment %}
{% assign nom= nom | where: "nominations" %}
{% endcomment %}

{% for nom in nominations %}
{% if nom.nominations %}
{{ nom.nominations.name }}

{% endif %}
{% endfor %}
```


