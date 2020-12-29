---
layout: post
title: Desktop App for 2-Factor Authentication
date: 2020-12-29
related_image: /assets/images/head.png
---

<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>
<small class="text-muted">Last updated {{ page.date | date: "%b %-d, %Y"}}</small>
<br/>
These days online security became a very challenging task. It is always a better idea to add an extra layer of security to our online accounts whenever it is possible. This will prevent others from accessing our accounts though they have got our password.

Most of the online services enforce their users to follow the best existing security practices. But sometimes, these best practices hinder our ease of access. For example, Dropbox / Gmail provides an extra layer of security through their 2-Factor Authentication. 


<p class="text-info">
How does this 2 Factor authentication works?
</p>
<p class="text-secondary">
2-Factor Authentication requires an extra device owned by the user to generate a random number. Using this random number, the provider will confirm the authentication for their application. But most of the providers like Google Authenticator or Microsoft Authenticator have mobile-only apps. This limitation restricts us from using the MFA to authenticate our services one or other time.
</p>

But, there is good news that I recently came across an App (Authy) that is available both on Mobile and Desktop (including Linux). Along with that, there are multiple advantages available. Please refer to their site https://support.authy.com/hc/en-us/articles/115001943608-Welcome-to-Authy-

Below is the screenshot for the Authy desktop app on my mac. This will help us when our mobile is not around or we lost it. Enjoy the comfort of the rapidly changing technology.  

<div class="text-center">
  <img src="/assets/images/2fa.png" class="rounded mx-auto d-block" alt="authy-app-on-mac">
</div>

<br/>
<p>
    <nav aria-label="Page navigation example">
      <ul class="pagination justify-content-center">
        <li class="page-item">
          <a class="page-link" href="/2020/11/19/go-concurrency.html" >Previous</a>
        </li>
        <li class="page-item disabled">
          <a class="page-link" href="#" tabindex="-1">Next</a>
        </li>
      </ul>
    </nav>
</p>

<div id="disqus_thread"></div>
<script>
   /*
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    
    */
    var disqus_config = function () {
    this.page.url = "https://www.parochi.xyz/2020/11/17/use-makefile-for-your-daily-development-tasks.html";  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = "20201117"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://parochi-xyz.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>