---
layout: default
title: Blog
---

 {% for post in site.posts %}
<div class="row row-cols-1 row-cols-md-3">
  <div class="col mb-10">
    <div class="card h-100">
      <!--Card image-->
      <div class="view overlay">
        <!-- <img class="card-img-top" src="{{ post.related_image }}" alt="Card image cap">-->
        <a href="#!">
          <div class="mask rgba-white-slight"></div>
        </a>
      </div>
      <!--Card content-->
      <div class="card-body">
        <!--Title-->
        <h4 class="card-title">{{ post.title }}</h4>
        <p class="card-title published-color">Published on {{ post.date | date: "%b %-d, %Y"}}</p>
        <!--Text-->
        <p class="card-text">{{ post.excerpt }}</p>
        <!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
        <button type="button" class="btn btn-md"><a href="{{ post.url }}" class="card-link">Read More...</a></button>
      </div>
    </div>
    <!-- Card -->
  </div>
</div>
<div>
&nbsp;
</div>
{% endfor %}