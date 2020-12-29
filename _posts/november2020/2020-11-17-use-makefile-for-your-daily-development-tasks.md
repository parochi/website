---
layout: post
title: Use Makefile for your daily development tasks
date: 2020-11-17
related_image: /assets/images/2020-11-17.jpg
---
<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>
<small class="text-muted">Last updated {{ page.date | date: "%b %-d, %Y"}}</small>
<br/>
A typical routine of a developer is code, build, unit test, and deploy. Most of the time, developers do all these tasks in their local first and once everything is fine then they move to the next state. This will be a very much repeated process when we work on a microservices architecture. 

From my experience, If I can list out a typical list of tasks for a go language-based microservice. Those are

1. Clean
2. Format
3. Lint
4. Dependencies
5. Run Unit tests
6. Build
7. Push to Dev Quay
8. Coverage Report

All these steps can be placed in a simple Makefile and then onwards, the same can be used by anyone to quickly take the charge and move on. This kind is one of the best practices when it comes to day-to-day development activity [I am not denying that there are multiple ways of achieving the same]. Here is the typical Makefile from my experience.

The below sample make file is specific to Go language-based environment but I believe the idea can be used to leverage with any other project. 

<pre><code class="Makefile">
# Enable dependencies and configure the private repo
GO = GO111MODULE=on GOPRIVATE=&lt;your private repo&gt;.cisco.com
EXEFILE = ./bin/&lt;exe file name&gt;
MAIN_GO = &lt;your service main.go file location&gt;

# help messages
help:
    @echo &#39;Available commands for the &lt;your micro service name&gt;&#39;
    @echo &#39;Usage&#39;
    @echo &#39;  all - Run all the available commands&#39;
    @echo &#39;  clean - Run the clean up&#39;
    @echo &#39;  setup -  Run the initial setup instructions&#39;
    @echo &#39;  format - Run code format&#39;
    @echo &#39;  staticanalysis - Run the static code analysis&#39;
    @echo &#39;  dependencies - Run the dependencies&#39;
    @echo &#39;  unittest - Run unit tests&#39;
    @echo &#39;  localbuild - Build the executable&#39;

# Run all the available commands in the below order
all: clean setup dependencies format staticanalysis unittest localbuild

clean:
    @echo &quot;Cleaning the previous executable and coverage reports&quot;
    rm -f coveragereport.out report.json
    rm -f ${EXEFILE }

setup:
    @echo &quot;Add any steps that are needed&quot;
    &lt;some installation tasks&gt;
format:
    @echo &quot;Code Formatting&quot;
    ${GO} fmt ./...
staticanalysis:
    @echo &quot;&quot;
    golangci-lint run -c .golangci.yaml
dependencies:
    @echo &#39; Load Dependencies&#39;
    ${GO} mod tidy
    ${GO} mod download
unittest:
    @echo &quot;Executing Unit tests&quot;
    ${GO} test -v covermode=atomic -count=1 ./... -coverprofile coveragereport.out
    ${GO} test -race test -covermode=atomic -count=1 ./... -json &gt; report.json
localbuild:
    @echo &quot;Loca build&quot;
    ${GO} build -o ${EXEFILE} ${MAIN_GO}
    docker build -t &lt;your tag name&gt; .
    docker tag &lt;your tag name&gt; &lt;your quay repo location&gt;:latest
    docker push &lt;your quay repo location&gt;:latest
</code></pre>

<br/>
<p>
    <nav aria-label="Page navigation">
      <ul class="pagination justify-content-center">
        <li class="page-item disabled">
          <a class="page-link" href="#" tabindex="-1">Previous</a>
        </li>
        <li class="page-item">
          <a class="page-link" href="/2020/11/18/day-to-day-kubernetes.html">Next</a>
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