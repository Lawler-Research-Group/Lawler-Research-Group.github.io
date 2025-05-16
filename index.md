---
layout: home # Or 'default', or a new layout you create e.g., 'frontpage'
title: Welcome to The Lawler Research Group # Or your site's main title
nav: true
nav_order: 1
permalink: /
hero_headline: Exploring the interface between condensed matter physics and quantum information science
hero_intro: We are interested in how quantum algorithms behave like condensed matter, how condensed matter is captured by quantum algorithms, and how machine learning can help us make discoveries in either of these areas.
hero_cta: Please explore our research, read our blog posts, or get to know our team!
description: The Lawler group research site seeking breakthroughs at the interface of condensed matter physics, quantum information science, and artificial intellegance.
news: true # includes a list of news items
selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page
---

<div class="hero-section" style="background-image: url('{{ site.baseurl }}/assets/img/your-optimized-hero-image.webp');">
  <div class="hero-content">
    <h1>{{ page.hero_headline | default: "Exploring the interface between condensed matter physics and quantum information science" }}</h1>
    <p class="lead">{{ page.hero_intro | default: "We are interested in how quantum algorithms behave like condensed matter, how condensed matter is captured by quantum algorithms, and how machine learning can help us make discoveries in either of these areas." }}</p>
    <p class="hero-cta">{{ page.hero_cta | default: "Please explore our research, read our blog posts, or get to know our team!" }}</p>
    </div>
</div>

<!--
<div class="hero-image-container">
  {% include figure.liquid path="assets/img/Lawler-Research-Group-Barren-Plateau.webp" alt="Barren plateau research highlight" class="img-fluid" %}
</div>

<div class="welcome-text-container" style="text-align: center; padding: 20px;">
  <h1>Exploring the interface between condensed matter physics and quantum information science</h1>
  <p class="lead">We are interested in how quantum algorithms behave like condensed matter, how condensed matter is captured by quantum algorithms, and how machine learning can help us make discoveries in either of these areas.</p>
  <p><strong>Please explore our research, read our blog posts, or get to know our team!</strong></p>
  <p>
      <a href="{{ '/research/' | relative_url }}" class="btn btn-primary">Explore Research</a>
      <a href="{{ '/blog/' | relative_url }}" class="btn btn-secondary">Read Our Blog</a>
      <a href="{{ '/people/' | relative_url }}" class="btn btn-secondary">Meet the Team</a>
  </p>
</div>
-->
