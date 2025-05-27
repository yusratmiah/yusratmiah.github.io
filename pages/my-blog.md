---
layout: default
title: My Blog
permalink: /my-blog.html
---

<div class="blog-post">
  <h1>📚 My Research Blog</h1>
  <p>Welcome to my daily research log for the CEAMLS SAIRI Summer 2025 program. Each entry reflects hands-on learning, challenges, and reflections.</p>
  <hr />

  <h2>📅 Table of Contents</h2>
<ul class="blog-toc">
  {% assign posts             = site.posts | sort: "date" %}
  {% assign seconds_per_day   = 86400 %}
  {% assign previous_monday   = "" %}
  {% assign week_counter      = 0 %}

{% for post in posts %}
{% assign post_ts    = post.date | date: "%s" %}
{% assign dow        = post.date | date: "%w" | plus: 0 %}

    {%- comment -%} calculate days to backtrack to Monday {%- endcomment -%}
    {% if dow == 0 %}
      {% assign back_days = 6 %}
    {% else %}
      {% assign back_days = dow | minus: 1 %}
    {% endif %}
    {% assign back_seconds       = back_days | times: seconds_per_day %}
    {% assign monday_ts          = post_ts | minus: back_seconds %}
    {% assign four_days_secs     = seconds_per_day | times: 4 %}
    {% assign friday_ts          = monday_ts | plus: four_days_secs %}

    {% capture monday_str %}{{ monday_ts  | date: "%B %-d" }}{% endcapture %}
    {% capture friday_str %}{{ friday_ts  | date: "%B %-d" }}{% endcapture %}

    {%- comment -%} when we hit a new monday, start a new week block and bump counter {%- endcomment -%}
    {% unless monday_ts == previous_monday %}
      {% assign week_counter    = week_counter | plus: 1 %}
      {% if previous_monday != "" %}
          </ul>
        </details>
      </li>
      {% endif %}
      <li>
        <details>
          <summary><strong>Week {{ week_counter }} ({{ monday_str }} – {{ friday_str }})</strong></summary>
          <ul>
      {% assign previous_monday = monday_ts %}
    {% endunless %}

    <li><a href="{{ post.url }}">{{ post.title }}</a></li>

    {% if forloop.last %}
          </ul>
        </details>
      </li>
    {% endif %}

{% endfor %}

</ul>
</div>
