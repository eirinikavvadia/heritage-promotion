---
layout: page
title: POIs List
permalink: /pois/
---

<h2>Courts of Greece</h2>
<p>Click on a court to view more information.</p>

<!-- Grid container for the court list -->
<div class="court-grid">
  {% for p in site.pois %}
    <a href="{{ p.url | relative_url }}" class="court-card" data-wikidatum="{{ p.wikidatum }}">
      <!-- Placeholder for image and title from Wikidata -->
      <div class="card-image" id="image-{{ p.wikidatum }}"></div>
      <div class="card-title" id="title-{{ p.wikidatum }}"></div>
    </a>
  {% endfor %}
</div>

<!-- Add CSS for the grid layout -->
<style>
  .court-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
    margin-top: 20px;
  }

  .court-card {
    text-decoration: none;
    color: inherit;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    transition: transform 0.3s;
  }

  .court-card:hover {
    transform: translateY(-5px);
  }

  .card-image {
    width: 100%;
    height: 150px;
    background-size: cover;
    background-position: center;
  }

  .card-title {
    padding: 10px;
    text-align: center;
    font-weight: bold;
    font-size: 16px;
  }
</style>
