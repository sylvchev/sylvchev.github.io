---
layout: page
permalink: /publications/
title: Publications
description: Publications in reversed chronological order. 
years: [2024, 2023, 2022, 2021,2020, 2019, 2018, 2017, 2016, 2015, 2014, 2013, 2012, 2011, 2010, 2009, 2008, 2006, 2005]
---

Full text from these publications are available on [HAL](https://cv.hal.science/sylvain-chevallier) and via my [google scholar profile](https://scholar.google.com/citations?user=j5Tu_SQAAAAJ&hl=fr).

<!-- <span class="pjournal">■</span> journals, <span class="pconf">■</span> conferences, <span class="pbook">■</span> books -->

<!-- <pjournal>■</pjournal> Journal, <pconf>■</pconf> Conference, <pbook>■</pbook> book chapters, thesis -->

{% for y in page.years %}
  <h3 class="year">{{y}}</h3>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}
