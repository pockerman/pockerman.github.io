---
layout: archive
title: "Big Data & Data Engineering"
permalink: /big-data/
author_profile: true
---

{% include base_path %}

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.bigdata %}
  {% include archive-single.html %}
{% endfor %}
