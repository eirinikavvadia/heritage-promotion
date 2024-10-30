---
layout: page
title: Εφετεία
permalink: /pois/
---

<h2>Τα Εφετεία της Ελλάδας</h2>
<p>Click για περσσότερες πληροφορίες</p>

<!-- Grid container -->
<div class="court-grid">
  {% for p in site.pois %}
    <a href="{{ p.url | relative_url }}" class="court-card" data-wikidatum="{{ p.wikidatum }}">
      <!-- Placeholder for image and title from Wikidata -->
      <div class="card-image" id="image-{{ p.wikidatum }}"></div>
      <div class="card-title">{{ p.title }}"></div>
    </a>
  {% endfor %}
</div>

<!-- CSS for the grid layout -->
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

<!-- Fetch Title and Image from Wikidata -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>
$(document).ready(function() {
  $('.court-card').each(function() {
    const wikidatum = $(this).data('wikidatum');
    get_wikidatum(wikidatum);
  });
});

function get_wikidatum(id) {
  let url = `https://www.wikidata.org/w/api.php?action=wbgetentities&ids=${id}&format=json&languages=en|el&origin=*`;
  $.getJSON(url, function(data) {
    const title = get_json_value(['entities', id, 'sitelinks', 'elwiki', 'title'], data);
    const image = get_json_value(['entities', id, 'claims', 'P18', 0, 'mainsnak', 'datavalue', 'value'], data);

    // Populate the title and image for each court card
    if (title) {
      $(`#title-${id}`).text(title);
    }

    if (image) {
      get_thumbnail(image, 300).then(thumbnailUrl => {
        $(`#image-${id}`).css('background-image', `url(${thumbnailUrl})`);
      });
    }
  });
}

function get_thumbnail(photoname, size) {
  return new Promise((resolve, reject) => {
    if (!photoname) return resolve('');  // Return empty if there's no image
    photoname = photoname.replace(/ /g, '_');
    const url = `https://api.wikimedia.org/core/v1/commons/file/File:${photoname}`;
    $.getJSON(url, function(data) {
      let thumbname = get_json_value(['thumbnail', 'url'], data);
      if (thumbname) {
        thumbname = thumbname.replace(/\/\d+px-/, `/${size}px-`);
        resolve(thumbname);
      } else {
        reject('Thumbnail not found');
      }
    }).fail(() => reject('Error fetching thumbnail'));
  });
}

// Function to access nested JSON data
function get_json_value(json_key, data) {
  try {
    while (json_key.length > 1) {
      data = data[json_key[0]];
      json_key = json_key.slice(1);
    }
    return data[json_key[0]];
  } catch (err) {
    console.error(err, json_key, JSON.stringify(data));
    return null;
  }
}
</script>
