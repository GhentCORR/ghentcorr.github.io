---
layout: default
permalink: /blog/
title: blog
nav: true
nav_order: 4
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="post">

{% assign blog_name_size = site.blog_name | size %}
{% assign blog_description_size = site.blog_description | size %}

{% if blog_name_size > 0 or blog_description_size > 0 %}
<div class="header-bar">
<h1 class="text-primary">
<span class="font-weight-bold">Ghent</span><span>CORR</span>
</h1>
<h2>{{ site.blog_description }}</h2>
</div>
{% endif %}

{% if site.display_tags and site.display_tags.size > 0 or site.display_categories and site.display_categories.size > 0 %}
<div class="dropdown my-4">
<select id="filterDropdown" class="form-control">
<option value="all">All Posts</option>
{% for tag in site.display_tags %}
<option value="{{ tag | slugify }}">{{ tag }}</option>
{% endfor %}
{% for category in site.display_categories %}
<option value="{{ category | slugify }}">{{ category }}</option>
{% endfor %}
</select>
</div>
{% endif %}

  <ul class="post-list" id="blogPostsList">
    {% if page.pagination.enabled %}
      {% assign postlist = paginator.posts %}
    {% else %}
      {% assign postlist = site.posts %}
    {% endif %}

    {% for post in postlist %}
      {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
      {% assign year = post.date | date: "%Y" %}
      {% assign tags = post.tags | join: "" %}
      {% assign categories = post.categories | join: "" %}

      <li class="post-item" data-tags="{{ post.tags | join: ' ' | slugify }}" data-categories="{{ post.categories | join: ' ' | slugify }}">
        {% if post.thumbnail %}
          <div class="row">
            <div class="col-sm-9">
        {% endif %}

        <h3>
          <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h3>
        <p>{{ post.description }}</p>
        <p class="post-meta">
          {{ read_time }} min read &nbsp; &middot; &nbsp;
          {{ post.date | date: '%B %d, %Y' }}
          {% if post.external_source %}
          &nbsp; &middot; &nbsp; {{ post.external_source }}
          {% endif %}
        </p>
        <p class="post-tags">
          <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}">
            <i class="fa-solid fa-calendar fa-sm"></i> {{ year }} </a>

          {% if tags != "" %}
          &nbsp; &middot; &nbsp;
            {% for tag in post.tags %}
            <a href="{{ tag | slugify | prepend: '/blog/tag/' | prepend: site.baseurl}}">
              <i class="fa-solid fa-hashtag fa-sm"></i> {{ tag }}</a>
              {% unless forloop.last %}
                &nbsp;
              {% endunless %}
              {% endfor %}
          {% endif %}

          {% if categories != "" %}
          &nbsp; &middot; &nbsp;
            {% for category in post.categories %}
            <a href="{{ category | slugify | prepend: '/blog/category/' | prepend: site.baseurl}}">
              <i class="fa-solid fa-tag fa-sm"></i> {{ category }}</a>
              {% unless forloop.last %}
                &nbsp;
              {% endunless %}
              {% endfor %}
          {% endif %}
        </p>

        {% if post.thumbnail %}
            </div>
            <div class="col-sm-3">
              <img class="card-img" src="{{ post.thumbnail | relative_url }}" style="object-fit: cover; height: 90%" alt="image">
            </div>
          </div>
        {% endif %}
      </li>
    {% endfor %}

  </ul>

{% if page.pagination.enabled %}
{% include pagination.liquid %}
{% endif %}

</div>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const dropdown = document.getElementById("filterDropdown");
    const postList = document.getElementById("blogPostsList");
    const postItems = postList.getElementsByClassName("post-item");

    dropdown.addEventListener("change", function () {
      const filterValue = dropdown.value;
      for (let i = 0; i < postItems.length; i++) {
        const item = postItems[i];
        const tags = item.getAttribute("data-tags");
        const categories = item.getAttribute("data-categories");

        if (filterValue === "all" || tags.includes(filterValue) || categories.includes(filterValue)) {
          item.style.display = "block";
        } else {
          item.style.display = "none";
        }
      }
    });
  });
</script>
