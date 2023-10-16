---
layout: page
permalink: /research/
title: Research
years: [2023,2022,2021]
pub: ["Conference Proceedings", "Workshops","Abstracts and Presentations"]
nav: true
nav_order: 1
---
<!-- _pages/research.md -->
<div class="publications">

* = equal contribution


{% for y in page.years %}
  <h4 class="year">{{y}}</h4>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}
</div>
