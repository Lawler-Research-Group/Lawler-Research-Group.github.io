---
layout: page
permalink: /publications/
title: publications
description:
years: [2021, 2020, 2019, 2018]
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  <!--{percent bibliography -f papers -q @*[year={{y}}]* percent} change percent to %-->
{% endfor %}

</div>
