---
layout: page
permalink: /publications/
title: Publications
description: For a complete/updated list of publications, please visit Michael's google scholar page.
years: [2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020, 2021, 2022]
nav: true
nav_order: 1
---
<!-- _pages/publications.md -->
<div class="publications">

{%- for y in page.years %}
  <h2 class="year">{{y}}</h2>
  <!--{percent bibliography -f papers -q @*[year={{y}}]* percent} change percent to %-->
{% endfor %}

</div>
