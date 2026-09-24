---
layout: page
permalink: /teaching/
title: teaching
description: Some courses I taught (mostly in French)
nav: true
nav_order: 3
---

<!-- Course pages are plain pages (layout: page) shown as cards, like projects.
     To use the v1 course layout instead (schedule tables, grouping by year),
     see "Creating a teachings collection" in docs/CUSTOMIZE.md. -->
<div class="projects">
{% assign sorted_courses = site.teachings | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_courses %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
