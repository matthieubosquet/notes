---
title: Notes
---

{% assign pages = site.pages | where_exp: "p", "p.permalink != '/'" %}
{% include tag-filtered-list.html content=pages %}

