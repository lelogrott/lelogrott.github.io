---
title: Categories
layout: default
permalink: /categories/
---

## Categories

<div>
{% for category in site.categories %}
  {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <h3 class="category-head">{{ category_name }}</h3>
    <a name="{{ category_name | slugize }}"></a>
    {% for post in site.categories[category_name] %}
      <a href="{{ post.url }}">{{post.title}}</a>
      <div class="post-stats">
        <i class="time-icon"></i>
        <span class="published-date">
          {{ post.date | date: "%b %-d, %Y" }}
        </span> |
        <a href="{{ post.url }}#disqus_thread">
          <i class="comment-icon"></i>
          <span class="disqus-comment-count" data-disqus-identifier="{{ post.disqus_identifier }}"></span>
        </a>
      </div>
      {% if forloop.last %}<br><br>{% endif %}
    {% endfor %}
{% endfor %}
</div>
