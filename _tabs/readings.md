---
# Readings: every post with a `source:` in its front matter (see `_includes/source-card.html`)
icon: fas fa-book-open
order: 1
title: Readings
---

{% assign readings = site.posts | where_exp: 'post', 'post.source' %}

<div id="readings">
  <p class="readings-intro">{% include t.html key="readings.intro" %}</p>

  {% if readings.size == 0 %}
    <p>{% include t.html key="readings.empty" %}</p>
  {% endif %}

  {% for post in readings %}
    {% assign src = post.source %}
    {% case src.type %}
      {% when 'paper' %}{% assign src_icon = 'fas fa-file-lines' %}
      {% when 'post' %}{% assign src_icon = 'fas fa-rss' %}
      {% when 'talk', 'video' %}{% assign src_icon = 'fas fa-circle-play' %}
      {% when 'book' %}{% assign src_icon = 'fas fa-book' %}
      {% else %}{% assign src_icon = 'far fa-newspaper' %}
    {% endcase %}
    {% capture src_type_key %}source.types.{{ src.type | default: 'article' }}{% endcapture %}

    <div class="reading">
      <div class="reading-type" title="{{ src.type | default: 'article' }}"><i class="{{ src_icon }} fa-fw"></i></div>
      <div class="reading-body">
        <a class="reading-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        {% if post.lang %}<span class="reading-lang">{{ post.lang | upcase }}</span>{% endif %}
        <div class="reading-source">
          {% include t.html key=src_type_key default=src.type %}:
          {{ src.title }}{% if src.authors %} · {{ src.authors }}{% endif %}{% if src.year %} ({{ src.year }}){% endif %}
        </div>
        <div class="reading-meta">
          {{ post.date | date: '%d/%m/%Y' }}
        </div>
      </div>
    </div>
  {% endfor %}
</div>
