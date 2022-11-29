---
layout: archive
title: "Writing Sample"
permalink: /writing-sample/
author_profile: true
---

{% include base_path %}
# Writing Sample
- abc
- def

{% for post in site.writing-sample reversed %}
  {% include archive-single.html %}
{% endfor %}
