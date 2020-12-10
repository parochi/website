---
layout: default
title: Blog
---

<div class="card-deck">
  <div class="card">
    <img class="card-img-top" src="{{ site.posts[0].related_image }}" alt="Card image cap">
    <div class="card-body">
      <h3 class="card-title"><a href="{{ site.posts[0].url }}" class="custom-card stretched-link">{{ site.posts[0].title }}</a></h3>
      <p class="card-text font-read-text-custom">One of the features that we can leverage, when we are working with Kubernetes is the ConfigMap. This is used to store the non-confidential information required by the pods. Do remember that, It is not suggested to store the confidential or secret information in the ConfigMaps. Let us see how to create a configmap using both imperative way and with the yaml file.</p>
    </div>
    <div class="card-footer">
      <small class="text-muted">Last updated {{ site.posts[0].date | date: "%b %-d, %Y"}}</small>
    </div>
  </div>
  
  
  <div class="card">
    <img class="card-img-top" src="{{ site.posts[1].related_image }}" alt="Card image cap">
    <div class="card-body">
      <h3 class="card-title"><a href="{{ site.posts[1].url }}" class="custom-card stretched-link">{{ site.posts[1].title }}</a></h3>
      <p class="card-text font-read-text-custom">Go handles concurrency in a slightly different way than the other programming languages. The effective Go has a slogan around their new concept "Do not communicate by sharing memory; instead, share memory by communicating". These Go routines are unique and they are not operating system threads. It is a function executing concurrently with other goroutines in the same address space.</p>
    </div>
    <div class="card-footer">
      <small class="text-muted">Last updated {{ site.posts[1].date | date: "%b %-d, %Y"}}</small>
    </div>
  </div>
  
  <div class="card">
    <img class="card-img-top" src="{{ site.posts[2].related_image }}" alt="Card image cap">
    <div class="card-body">
      <h3 class="card-title"><a href="{{ site.posts[2].url }}" class="custom-card stretched-link">{{ site.posts[2].title }}</a></h3>
      <p class="card-text font-read-text-custom">There are multiple resources for the Kubernetes `kubectl` command line reference. It is very good practice to learn the usage of all those commands. But, here I would like to share the most day to day commands that we use on the terminal to take care of our daily development and debugging activities with respect to Kubernetes.</p>
    </div>
    <div class="card-footer">
      <small class="text-muted">Last updated {{ site.posts[2].date | date: "%b %-d, %Y"}}</small>
    </div>
  </div>
  
</div>

<div>
&nbsp;
<!-- You can place your ad here-->
<div>

<div class="card-deck">
  <!--
  <div class="card">
    <img class="card-img-top" src="{{ site.posts[3].related_image }}" alt="Card image cap">
    <div class="card-body">
      <h3 class="card-title"><a href="{{ site.posts[3].url }}" class="custom-card stretched-link">{{ site.posts[3].title }}</a></h3>
      <p class="card-text font-read-text-custom">A typical routine of a developer is code, build, unit test, and deploy. Most of the time, developers do all these tasks in their local first and once everything is fine then they move to the next state. This will be a very much repeated process when we work on a microservices architecture. From my experience, If I can list out a typical list of tasks for a go language-based microservice. Those are</p>
    </div>
    <div class="card-footer">
      <small class="text-muted">Last updated {{ site.posts[3].date | date: "%b %-d, %Y"}}</small>
    </div>
  </div>
  -->
</div>