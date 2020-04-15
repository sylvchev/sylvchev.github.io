---
layout: page
permalink: /publications/
title: Publications
description: Publications in reversed chronological order. 
years: [2019, 2018, 2017, 2016, 2015, 2014, 2013, 2012, 2011, 2010, 2009, 2008, 2006, 2005]
---

Full text from these publications are available on [HAL](https://cv.archives-ouvertes.fr/sylvain-chevallier) and via my [google scholar profile](https://scholar.google.com/citations?user=j5Tu_SQAAAAJ&hl=fr).


{% for y in page.years %}
  <h3 class="year">{{y}}</h3>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}
