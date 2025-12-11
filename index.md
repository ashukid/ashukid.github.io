---
layout: home
title: About Me
---


<section class="home-about">
  <p>I'm Swastika Mukherjee, a content writer who turns rough ideas into clear, credible narratives. I work with founders, marketers, and creators to shape blog posts, thought-leadership pieces, scripts, and email/social copy that feel helpful—not salesy.</p>

  <p>My process starts with research and empathy: talking to subject-matter experts, understanding audience pain points, and mapping the story so it’s easy to skim yet grounded in substance. I care about structure, clean language, and visuals that support the message.</p>

  <p>I’m motivated by clarity and reader trust—strong intros, logical flow, and concrete takeaways that respect people’s time. When a piece feels effortless to read and actually helps someone, that’s the win.</p>
</section>

<section class="home-blogs">
  <div class="home-blogs-header">
    <h2>Recent Blogs</h2>
    <a class="button" href="{{ '/blogs/' | relative_url }}">View all</a>
  </div>

  {%- if site.posts.size > 0 -%}
    <div class="home-blogs-list">
      {%- for post in site.posts limit:3 -%}
      <a class="home-blog-item" href="{{ post.url | relative_url }}">
        <div class="home-blog-image">
          {%- if post.image -%}
          <img src="{{ post.image }}" alt="{{ post.title | escape }}" loading="lazy">
          {%- else -%}
          <div class="home-blog-placeholder">✦</div>
          {%- endif -%}
        </div>
        <div class="home-blog-content">
          <h3>{{ post.title | escape }}</h3>
          {%- if post.excerpt -%}
          <p>{{ post.excerpt | strip_html | truncatewords: 26 }}</p>
          {%- endif -%}
          <span class="home-blog-date">
            {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
            {{ post.date | date: date_format }}
          </span>
        </div>
      </a>
      {%- endfor -%}
    </div>
  {%- endif -%}
</section>
