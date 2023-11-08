<h4>News</h4>

{% assign sitedate = site.time | date: "%Y-%m-%d"   %}
<table width="60%">
{% for post in site.posts %}

{% assign stopdate = post.until | string_to_date | date: "%Y-%m-%d" %}
{% if stopdate > sitedate %}

<tr>
  <td>
<!-- div class="row alert alert-news" -->
  <!-- p class="lead event-announce" -->
    <a href="{{ post.url }}">
      <span class="glyphicon {{ post.symbol }}" aria-hidden="true"></span> {{ post.title }}
    </a>
  <!-- /p -->
<!-- /div -->
  </td>
  <td>
    {% assign post_date=post.date | date: "%Y-%m-%d" %}
    {{ post_date }}
  </td>
  </tr>
{% endif %}

{% endfor %}
</table>
