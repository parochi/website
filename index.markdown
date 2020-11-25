---
layout: default
title: Blog
---

<div class="container">
    <div class="row">
        <div class="col-12">      
                
                {% for post in site.posts %}
                <div class="row">
                    <div class="col-sm-12">
                        <div class="card bg-light mb-3">
                          <div class="card-body">
                            <h4 class="card-title">{{ post.title }}</h4>
                            <p class="card-text">{{ post.excerpt }}</p>
                            <a href="{{ post.url }}" class="card-link">Read More...</a>
                          </div>
                        </div>
                    </div>
                </div>
                {% endfor %}
                
        </div>
    </div>
</div>
